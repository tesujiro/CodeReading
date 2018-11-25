# Repository
https://github.com/Azure/golua

# Run

# basic code path


# Where is the lexer?

# Where is the parser?

# Where is the VM?

# Structure
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

