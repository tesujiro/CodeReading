# Repository
https://github.com/Azure/golua

# Run

# basic code path


# Where is the lexer?

# Where is the parser?

# Where is the VM?

# Structure
<!DOCTYPE html>
<html>
<head>
 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
 <meta name="Author" content="Made by 'tree'">
 <meta name="GENERATOR" content="$Version: $ tree v1.8.0 (c) 1996 - 2018 by Steve Baker, Thomas Moore, Francesc Rocher, Florian Sesser, Kyosuke Tokoro $">
 <title>Directory Tree</title>
 <style type="text/css">
  <!--
  BODY { font-family : ariel, monospace, sans-serif; }
  P { font-weight: normal; font-family : ariel, monospace, sans-serif; color: black; background-color: transparent;}
  B { font-weight: normal; color: black; background-color: transparent;}
  A:visited { font-weight : normal; text-decoration : none; background-color : transparent; margin : 0px 0px 0px 0px; padding : 0px 0px 0px 0px; display: inline; }
  A:link    { font-weight : normal; text-decoration : none; margin : 0px 0px 0px 0px; padding : 0px 0px 0px 0px; display: inline; }
  A:hover   { color : #000000; font-weight : normal; text-decoration : underline; background-color : yellow; margin : 0px 0px 0px 0px; padding : 0px 0px 0px 0px; display: inline; }
  A:active  { color : #000000; font-weight: normal; background-color : transparent; margin : 0px 0px 0px 0px; padding : 0px 0px 0px 0px; display: inline; }
  .VERSION { font-size: small; font-family : arial, sans-serif; }
  .NORM  { color: black;  background-color: transparent;}
  .FIFO  { color: purple; background-color: transparent;}
  .CHAR  { color: yellow; background-color: transparent;}
  .DIR   { color: blue;   background-color: transparent;}
  .BLOCK { color: yellow; background-color: transparent;}
  .LINK  { color: aqua;   background-color: transparent;}
  .SOCK  { color: fuchsia;background-color: transparent;}
  .EXEC  { color: green;  background-color: transparent;}
  -->
 </style>
</head>
<body>
	<h1>Directory Tree</h1><p>
	<a href="aaa">aaa</a><br>
	├── <a href="aaa/LICENSE">LICENSE</a><br>
	├── <a href="aaa/README.md">README.md</a><br>
	├── <a href="aaa/cmd/">cmd</a><br>
	│   └── <a href="aaa/cmd/glua/">glua</a><br>
	│   &nbsp;&nbsp;&nbsp; └── <a href="aaa/cmd/glua/main.go">main.go</a><br>
	├── <a href="aaa/glua">glua</a><br>
	├── <a href="aaa/hello.lua">hello.lua</a><br>
	├── <a href="aaa/lua/">lua</a><br>
	│   ├── <a href="aaa/lua/access.go">access.go</a><br>
	│   ├── <a href="aaa/lua/auxiliary.go">auxiliary.go</a><br>
	│   ├── <a href="aaa/lua/binary/">binary</a><br>
	│   │   ├── <a href="aaa/lua/binary/binary.go">binary.go</a><br>
	│   │   ├── <a href="aaa/lua/binary/binutil.go">binutil.go</a><br>
	│   │   ├── <a href="aaa/lua/binary/decode.go">decode.go</a><br>
	│   │   └── <a href="aaa/lua/binary/encode.go">encode.go</a><br>
	│   ├── <a href="aaa/lua/closure.go">closure.go</a><br>
	│   ├── <a href="aaa/lua/config.go">config.go</a><br>
	│   ├── <a href="aaa/lua/consts.go">consts.go</a><br>
	│   ├── <a href="aaa/lua/debug.go">debug.go</a><br>
	│   ├── <a href="aaa/lua/errors.go">errors.go</a><br>
	│   ├── <a href="aaa/lua/event.go">event.go</a><br>
	│   ├── <a href="aaa/lua/exec.go">exec.go</a><br>
	│   ├── <a href="aaa/lua/frame.go">frame.go</a><br>
	│   ├── <a href="aaa/lua/lua.go">lua.go</a><br>
	│   ├── <a href="aaa/lua/lvm.go">lvm.go</a><br>
	│   ├── <a href="aaa/lua/ops.go">ops.go</a><br>
	│   ├── <a href="aaa/lua/stack.go">stack.go</a><br>
	│   ├── <a href="aaa/lua/state.go">state.go</a><br>
	│   ├── <a href="aaa/lua/syntax/">syntax</a><br>
	│   │   ├── <a href="aaa/lua/syntax/source.go">source.go</a><br>
	│   │   └── <a href="aaa/lua/syntax/string.go">string.go</a><br>
	│   ├── <a href="aaa/lua/table.go">table.go</a><br>
	│   ├── <a href="aaa/lua/value.go">value.go</a><br>
	│   └── <a href="aaa/lua/vm/">vm</a><br>
	│   &nbsp;&nbsp;&nbsp; ├── <a href="aaa/lua/vm/code.go">code.go</a><br>
	│   &nbsp;&nbsp;&nbsp; ├── <a href="aaa/lua/vm/instr.go">instr.go</a><br>
	│   &nbsp;&nbsp;&nbsp; ├── <a href="aaa/lua/vm/mask.go">mask.go</a><br>
	│   &nbsp;&nbsp;&nbsp; └── <a href="aaa/lua/vm/mode.go">mode.go</a><br>
	├── <a href="aaa/main">main</a><br>
	├── <a href="aaa/pkg/">pkg</a><br>
	│   ├── <a href="aaa/pkg/packer/">packer</a><br>
	│   │   ├── <a href="aaa/pkg/packer/option.go">option.go</a><br>
	│   │   ├── <a href="aaa/pkg/packer/packer.go">packer.go</a><br>
	│   │   ├── <a href="aaa/pkg/packer/scan.go">scan.go</a><br>
	│   │   └── <a href="aaa/pkg/packer/state.go">state.go</a><br>
	│   ├── <a href="aaa/pkg/pattern/">pattern</a><br>
	│   │   ├── <a href="aaa/pkg/pattern/classes.go">classes.go</a><br>
	│   │   ├── <a href="aaa/pkg/pattern/compile.go">compile.go</a><br>
	│   │   ├── <a href="aaa/pkg/pattern/matcher.go">matcher.go</a><br>
	│   │   ├── <a href="aaa/pkg/pattern/pattern.go">pattern.go</a><br>
	│   │   ├── <a href="aaa/pkg/pattern/replacer.go">replacer.go</a><br>
	│   │   └── <a href="aaa/pkg/pattern/scanner.go">scanner.go</a><br>
	│   └── <a href="aaa/pkg/strings/">strings</a><br>
	│   &nbsp;&nbsp;&nbsp; ├── <a href="aaa/pkg/strings/all_test.go">all_test.go</a><br>
	│   &nbsp;&nbsp;&nbsp; ├── <a href="aaa/pkg/strings/strings.go">strings.go</a><br>
	│   &nbsp;&nbsp;&nbsp; └── <a href="aaa/pkg/strings/wrapper.go">wrapper.go</a><br>
	├── <a href="aaa/std/">std</a><br>
	│   ├── <a href="aaa/std/base/">base</a><br>
	│   │   └── <a href="aaa/std/base/base.go">base.go</a><br>
	│   ├── <a href="aaa/std/coro/">coro</a><br>
	│   │   └── <a href="aaa/std/coro/coro.go">coro.go</a><br>
	│   ├── <a href="aaa/std/debug/">debug</a><br>
	│   │   └── <a href="aaa/std/debug/debug.go">debug.go</a><br>
	│   ├── <a href="aaa/std/io/">io</a><br>
	│   │   ├── <a href="aaa/std/io/file.go">file.go</a><br>
	│   │   ├── <a href="aaa/std/io/io.go">io.go</a><br>
	│   │   └── <a href="aaa/std/io/stream.go">stream.go</a><br>
	│   ├── <a href="aaa/std/math/">math</a><br>
	│   │   └── <a href="aaa/std/math/math.go">math.go</a><br>
	│   ├── <a href="aaa/std/os/">os</a><br>
	│   │   └── <a href="aaa/std/os/os.go">os.go</a><br>
	│   ├── <a href="aaa/std/pkg/">pkg</a><br>
	│   │   └── <a href="aaa/std/pkg/pkg.go">pkg.go</a><br>
	│   ├── <a href="aaa/std/std.go">std.go</a><br>
	│   ├── <a href="aaa/std/str/">str</a><br>
	│   │   ├── <a href="aaa/std/str/format.go">format.go</a><br>
	│   │   ├── <a href="aaa/std/str/string.go">string.go</a><br>
	│   │   └── <a href="aaa/std/str/strutil.go">strutil.go</a><br>
	│   ├── <a href="aaa/std/table/">table</a><br>
	│   │   └── <a href="aaa/std/table/table.go">table.go</a><br>
	│   └── <a href="aaa/std/utf8/">utf8</a><br>
	│   &nbsp;&nbsp;&nbsp; └── <a href="aaa/std/utf8/utf8.go">utf8.go</a><br>
	└── <a href="aaa/test/">test</a><br>
	&nbsp;&nbsp;&nbsp; ├── <a href="aaa/test/README">README</a><br>
	&nbsp;&nbsp;&nbsp; ├── <a href="aaa/test/lua-5.3.4/">lua-5.3.4</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/all.lua">all.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/api.lua">api.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/attrib.lua">attrib.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/big.lua">big.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/bitwise.lua">bitwise.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/calls.lua">calls.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/closure.lua">closure.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/code.lua">code.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/constructs.lua">constructs.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/coroutine.lua">coroutine.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/db.lua">db.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/errors.lua">errors.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/events.lua">events.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/files.lua">files.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/gc.lua">gc.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/goto.lua">goto.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/literals.lua">literals.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/locals.lua">locals.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/main.lua">main.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/math.lua">math.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/nextvar.lua">nextvar.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/pm.lua">pm.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/sort.lua">sort.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/strings.lua">strings.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/tpack.lua">tpack.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/utf8.lua">utf8.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   ├── <a href="aaa/test/lua-5.3.4/vararg.lua">vararg.lua</a><br>
	&nbsp;&nbsp;&nbsp; │   └── <a href="aaa/test/lua-5.3.4/verybig.lua">verybig.lua</a><br>
	&nbsp;&nbsp;&nbsp; └── <a href="aaa/test/test.sh">test.sh</a><br>
	<br><br>
	</p>
	<p>

23 directories, 91 files
	<br><br>
	</p>
	<hr>
	<p class="VERSION">
		 tree v1.8.0 © 1996 - 2018 by Steve Baker and Thomas Moore <br>
		 HTML output hacked and copyleft © 1998 by Francesc Rocher <br>
		 JSON output hacked and copyleft © 2014 by Florian Sesser <br>
		 Charsets / OS/2 support © 2001 by Kyosuke Tokoro
	</p>
</body>
</html>


