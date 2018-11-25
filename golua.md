# Repository
https://github.com/Azure/golua

# Run

# basic code path


# Where is the lexer?

# Where is the parser?

# Where is the VM?

# Structure
```XML
<?xml version="1.0" encoding="UTF-8"?>
<tree>
  <directory name=".">
    <directory name="cmd">
      <directory name="glua">
        <file name="main.go"></file>
      </directory>
    </directory>
    <directory name="lua">
      <file name="access.go"></file>
      <file name="auxiliary.go"></file>
      <directory name="binary">
        <file name="binary.go"></file>
        <file name="binutil.go"></file>
        <file name="decode.go"></file>
        <file name="encode.go"></file>
      </directory>
      <file name="closure.go"></file>
      <file name="config.go"></file>
      <file name="consts.go"></file>
      <file name="debug.go"></file>
      <file name="errors.go"></file>
      <file name="event.go"></file>
      <file name="exec.go"></file>
      <file name="frame.go"></file>
      <file name="lua.go"></file>
      <file name="lvm.go"></file>
      <file name="ops.go"></file>
      <file name="stack.go"></file>
      <file name="state.go"></file>
      <directory name="syntax">
        <file name="source.go"></file>
        <file name="string.go"></file>
      </directory>
      <file name="table.go"></file>
      <file name="value.go"></file>
      <directory name="vm">
        <file name="code.go"></file>
        <file name="instr.go"></file>
        <file name="mask.go"></file>
        <file name="mode.go"></file>
      </directory>
    </directory>
    <directory name="pkg">
      <directory name="packer">
        <file name="option.go"></file>
        <file name="packer.go"></file>
        <file name="scan.go"></file>
        <file name="state.go"></file>
      </directory>
      <directory name="pattern">
        <file name="classes.go"></file>
        <file name="compile.go"></file>
        <file name="matcher.go"></file>
        <file name="pattern.go"></file>
        <file name="replacer.go"></file>
        <file name="scanner.go"></file>
      </directory>
      <directory name="strings">
        <file name="all_test.go"></file>
        <file name="strings.go"></file>
        <file name="wrapper.go"></file>
      </directory>
    </directory>
    <directory name="std">
      <directory name="base">
        <file name="base.go"></file>
      </directory>
      <directory name="coro">
        <file name="coro.go"></file>
      </directory>
      <directory name="debug">
        <file name="debug.go"></file>
      </directory>
      <directory name="io">
        <file name="file.go"></file>
        <file name="io.go"></file>
        <file name="stream.go"></file>
      </directory>
      <directory name="math">
        <file name="math.go"></file>
      </directory>
      <directory name="os">
        <file name="os.go"></file>
      </directory>
      <directory name="pkg">
        <file name="pkg.go"></file>
      </directory>
      <file name="std.go"></file>
      <directory name="str">
        <file name="format.go"></file>
        <file name="string.go"></file>
        <file name="strutil.go"></file>
      </directory>
      <directory name="table">
        <file name="table.go"></file>
      </directory>
      <directory name="utf8">
        <file name="utf8.go"></file>
      </directory>
    </directory>
    <directory name="test">
      <directory name="lua-5.3.4">
      </directory>
    </directory>
  </directory>
  <report>
    <directories>23</directories>
    <files>56</files>
  </report>
</tree>
```

