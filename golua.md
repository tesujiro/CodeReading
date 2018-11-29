# Repository
https://github.com/Azure/golua

# Run

# Basic code path 1 : main - compile - load to chunk - new closure
cmd/glua/main.go#main
- lua/lua.go#State.Main
  - lua/auxiliary.go#State.ExecFile(file string)
    - lua/lua.go#State.ExecChunk
      - lua/lua.go#state.LoadChunk
        - lua/state.go#state.load() *Closure
	  - cmd := exec.Command("luac", "-o", tmp, "-")
	  - chunk, err := binary.Load(src)
	    - lua/binary/binary.go Load(src)
	      - lua/binary/decode.go decode(bytes.NewBuffer(data), &chunk)
	        - set Chunk.Header
		- decodePrototype(r, &chunk.Entry)
	          - // decode line start
	          - // decode line end
		  - // decode number of parameters
		  - // decode is varadic
		  - // decode maximum stack size
		  - // decode instruction bytecode
		  - // decode constants
		  - // decode upvalues
		  - // decode nested closure prototypes
		  - // decode line info (pc -> line)
		  - // decode local variables
		  - // decode upvalue names
	  - cls := newLuaClosure(&chunk.Entry)
	    - lua/closure.go newLuaClosure(proto *binary.Prototype) *Closure
	      - cls := &Closure{binary: proto}
	      - cls.upvals = make([]*upValue, len(proto.UpValues))  <-- upvals: to save values of parent function 
	- state.frame().push(cls)
      - lua/lua.go#state.Call <- call a function on the stack with arguments

# Basic code path 2: call a function
```
// To call a function you must use the following protocol: first, the function to be called is pushed onto the stack;
// then, the arguments to the function are pushed in direct order; that is, the first argument is pushed first.
// Finally you call lua_call; nargs is the number of arguments that you pushed onto the stack. All arguments and the
// function value are popped from the stack when the function is called. The function results are pushed onto the stack
// when the function returns. The number of results is adjusted to nresults, unless nresults is LUA_MULTRET.
// In this case, all results from the function are pushed; Lua takes care that the returned values fit into the stack
// space, but it does not ensure any extra space in the stack. The function results are pushed onto the stack in direct
// order (the first result is pushed first), so that after the call the last result is on the top of the stack.
```
lua/lua.go#state.Call <- call a function on the stack with arguments
  - funcID = state.frame().absindex(-(args + 1))
  - value  = state.frame().get(funcID - 1)
  - c, ok  = value.(\*Closure)
  - state.call(&Frame{closure: c, fnID: funcID, rets: rets})
    - lua/state.go call

```
// Calls a function (Go or Lua). The function to be called is at funcID in the stack.
// The arguments are on the stack in direct order following the function.
//
// On return, all the results are on the stack, starting at the original function position.
```
lua/lua.go#state.call (fr \*Frame) {
  args := state.frame().popN(state.frame().gettop() - fr.fnID + 1)[1:]
  fr.pushN(args)
  if fr.function().isLua() {
    execute(&v53{state})
  } else if fr.function().isGo() {
    rets := fr.popN(fr.function().native(state))
    fr.caller().pushN(rets)
  }
  return

# Basic code path 3: execute
``` golang:lua/exec.go
func execute(vm \*v53) {
	for cmd, instr := vm.fetch(); cmd != nil; cmd, instr = cmd(vm, instr) {
		vm.trace(instr)
	}
}

// fetch returns the next opcode function and instruction to execute
// incrementing the frame's instruction pointer (pc).
func (vm \*v53) fetch() (cmd, vm.Instr) {
	i := vm.thread().frame().step(1)
	return ops[i.Code()], i
}

// cmd is an executor for a lua opcode.
type cmd func(\*v53, vm.Instr) (cmd, vm.Instr)

// ops is a table of lua v53 opcode commands.
var ops []cmd

func init() {
	ops = []cmd{
		vm.MOVE: func(vm *v53, instr vm.Instr) (cmd, vm.Instr) {
			vm.move(instr)
			return vm.fetch()
		},
		vm.LOADK: func(vm *v53, instr vm.Instr) (cmd, vm.Instr) {
			vm.loadk(instr)
			return vm.fetch()
		},
		,
		,
		,
		vm.EXTRAARG: func(vm *v53, instr vm.Instr) (cmd, vm.Instr) {
			vm.extraarg(instr)
			return vm.fetch()
		},
	}
}
```

# Where is the parser?
There is no parser, this project is a implementation of Lua VM.
In lua/state.go, "luac" is used for compiling source to bytecode.

# Where is the VM?

# key part of data structure

## lua/closure.go
```
Closure struct {
	binary *binary.Prototype
	native Func
	upvals []*upValue
}
upValue struct {
	frame *Frame // frame upValue was opened within.
	ident string // name if debugging enabled.
	local bool   // true if in frame local's stack.
	index int    // index into stack or enclosing function.
	value Value  // if closed.
}
```

## lua/binary
```
Prototype struct {
	Source   string
	SrcPos   uint32
	EndPos   uint32
	Params   byte
	Vararg   byte
	Stack    byte
	Code     []uint32
	Consts   []interface{}
	UpValues []UpValue
	Protos   []Prototype
	PcLnTab  []uint32
	Locals   []LocalVar
	UpNames  []string
}

Header struct {
	Signature  [4]byte
	Version    byte
	Format     byte
	LuacData   [6]byte
	GoIntSize  byte
	SizetSize  byte
	InstrSize  byte
	LuaIntSize byte
	LuaNumSize byte
	LuacIntEnc int64
	LuacNumEnc float64
}

LocalVar struct {
	Name string
	Live uint32
	Dead uint32
}

UpValue struct {
	InStack byte
	Index   byte
}

Chunk struct {
	Header Header
	Entry  Prototype
}

```

# Instruction set of Lua VM

```
MOVE	A B R(A) := R(B)
LOADK	A Bx R(A) := K(Bx)
LOADBOOL A B C R(A) := (Bool)B; if (C) PC++
LOADNIL A B R(A) := ... := R(B) := nil
GETUPVAL A B R(A) := U[B]
GETGLOBAL A Bx R(A) := G[K(Bx)]
GETTABLE A B C R(A) := R(B)[RK(C)]
SETGLOBAL A Bx G[K(Bx)] := R(A)
SETUPVAL A B U[B] := R(A)
SETTABLE A B C R(A)[RK(B)] := RK(C)
NEWTABLE A B C R(A) := {} (size = B,C)
SELF A B C R(A+1) := R(B); R(A) := R(B)[RK(C)]
ADD A B C R(A) := RK(B) + RK(C)
SUB A B C R(A) := RK(B) - RK(C)
MUL A B C R(A) := RK(B) * RK(C)
DIV A B C R(A) := RK(B) / RK(C)
POW A B C R(A) := RK(B) ^ RK(C)
UNM A B R(A) := -R(B)
NOT A B R(A) := not R(B)
CONCAT A B C R(A) := R(B) .. ... .. R(C)
JMP sBx PC += sBx
EQ A B C if ((RK(B) == RK(C)) ~= A) then PC++
LT A B C if ((RK(B) < RK(C)) ~= A) then PC++
LE A B C if ((RK(B) <= RK(C)) ~= A) then PC++
TEST A B C if (R(B) <=> C) then R(A) := R(B) else PC++
CALL A B C R(A), ... ,R(A+C-2) := R(A)(R(A+1), ... ,R(A+B-1))
TAILCALL A B C return R(A)(R(A+1), ... ,R(A+B-1))
RETURN A B return R(A), ... ,R(A+B-2) (see note)
FORLOOP A sBx R(A)+=R(A+2); if R(A) <?= R(A+1) then PC+= sBx
TFORLOOP A C R(A+2), ... ,R(A+2+C) := R(A)(R(A+1), R(A+2));
TFORPREP A sBx if type(R(A)) == table then R(A+1):=R(A), R(A):=next;
SETLIST A Bx R(A)[Bx-Bx%FPF+i] := R(A+i), 1 <= i <= Bx%FPF+1
SETLISTO A Bx
CLOSE A close stack variables up to R(A)
CLOSURE A Bx R(A) := closure(KPROTO[Bx], R(A), ... ,R(A+n))
```

# Files
```
.
├── [  96]  cmd
│   └── [  96]  glua
│       └── [ 804]  main.go
├── [ 736]  lua
│   ├── [ 12K]  access.go
│   ├── [8.1K]  auxiliary.go
│   ├── [ 192]  binary
│   │   ├── [3.0K]  binary.go
│   │   ├── [2.8K]  binutil.go
│   │   ├── [4.4K]  decode.go
│   │   └── [ 474]  encode.go
│   ├── [2.1K]  closure.go
│   ├── [5.2K]  config.go
│   ├── [1.5K]  consts.go
│   ├── [ 10K]  debug.go
│   ├── [1.4K]  errors.go
│   ├── [ 18K]  event.go
│   ├── [7.4K]  exec.go
│   ├── [9.8K]  frame.go
│   ├── [ 23K]  lua.go
│   ├── [ 20K]  lvm.go
│   ├── [ 17K]  ops.go
│   ├── [5.9K]  stack.go
│   ├── [ 16K]  state.go
│   ├── [ 128]  syntax
│   │   ├── [ 650]  source.go
│   │   └── [3.3K]  string.go
│   ├── [5.1K]  table.go
│   ├── [4.9K]  value.go
│   └── [ 192]  vm
│       ├── [1.5K]  code.go
│       ├── [1.9K]  instr.go
│       ├── [3.5K]  mask.go
│       └── [ 244]  mode.go
├── [ 160]  pkg
│   ├── [ 192]  packer
│   │   ├── [ 197]  option.go
│   │   ├── [2.1K]  packer.go
│   │   ├── [5.5K]  scan.go
│   │   └── [2.1K]  state.go
│   ├── [ 256]  pattern
│   │   ├── [2.1K]  classes.go
│   │   ├── [3.1K]  compile.go
│   │   ├── [2.5K]  matcher.go
│   │   ├── [2.2K]  pattern.go
│   │   ├── [ 476]  replacer.go
│   │   └── [5.3K]  scanner.go
│   └── [ 160]  strings
│       ├── [2.2K]  all_test.go
│       ├── [6.3K]  strings.go
│       └── [4.8K]  wrapper.go
├── [ 416]  std
│   ├── [  96]  base
│   │   └── [ 17K]  base.go
│   ├── [  96]  coro
│   │   └── [1.3K]  coro.go
│   ├── [  96]  debug
│   │   └── [ 15K]  debug.go
│   ├── [ 160]  io
│   │   ├── [6.0K]  file.go
│   │   ├── [7.9K]  io.go
│   │   └── [2.6K]  stream.go
│   ├── [  96]  math
│   │   └── [ 11K]  math.go
│   ├── [  96]  os
│   │   └── [9.3K]  os.go
│   ├── [  96]  pkg
│   │   └── [9.6K]  pkg.go
│   ├── [1.1K]  std.go
│   ├── [ 160]  str
│   │   ├── [6.3K]  format.go
│   │   ├── [ 16K]  string.go
│   │   └── [2.8K]  strutil.go
│   ├── [  96]  table
│   │   └── [7.8K]  table.go
│   └── [  96]  utf8
│       └── [7.6K]  utf8.go
└── [ 160]  test
    └── [ 992]  lua-5.3.4
```

