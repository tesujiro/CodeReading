# Repository
- SQL-like query language for csv 
- https://github.com/mithrandie/csvq
- https://mithrandie.github.io/csvq

# Example1
```
tesujiro$ cat <<EOF | csvq "select key,value where key>5"
> key,value
> 1,aaa
> 3,bbb
> 5,ccc
> 7,ddd
> 9,eee
> EOF
+-----+-------+
| key | value |
+-----+-------+
| 7   | ddd   |
| 9   | eee   |
+-----+-------+
tesujiro$
```

# Sources
```
tesujiro$ tree -h -I docs -d
.
├── [ 352]  lib
│   ├── [ 416]  action
│   ├── [ 416]  cmd
│   ├── [ 224]  excmd
│   ├── [ 352]  file
│   ├── [ 640]  json
│   ├── [ 448]  parser
│   ├── [2.2K]  query
│   ├── [ 224]  syntax
│   └── [ 256]  value
└── [ 160]  testdata
    └── [ 672]  csv

12 directories
$ find . -name \*.go | grep -v _test.go | xargs wc -l
      17 ./init_windows.go
     194 ./lib/cmd/default_env.go
      74 ./lib/cmd/palette.go
     499 ./lib/cmd/flags.go
      31 ./lib/cmd/static.go
     198 ./lib/cmd/environment.go
     399 ./lib/cmd/utils.go
     371 ./lib/file/handler.go
      11 ./lib/file/config.go
      83 ./lib/file/error.go
      35 ./lib/file/functions.go
      50 ./lib/file/container.go
     214 ./lib/value/type.go
     304 ./lib/value/comparison.go
     345 ./lib/value/conv.go
     523 ./lib/parser/scanner.go
      29 ./lib/parser/format_identifiers.go
      66 ./lib/parser/lexer.go
    1515 ./lib/parser/ast.go
    4503 ./lib/parser/parser.go
      30 ./lib/action/fields.go
      27 ./lib/action/signal.go
     232 ./lib/action/run.go
     166 ./lib/action/update.go
      34 ./lib/action/syntax.go
      53 ./lib/action/calc.go
     208 ./lib/json/query.go
       8 ./lib/json/path_structure.go
     676 ./lib/json/query_parser.go
      85 ./lib/json/query_structure.go
      36 ./lib/json/cache.go
     185 ./lib/json/conversion.go
     122 ./lib/json/path_scanner.go
     155 ./lib/json/query_scanner.go
     462 ./lib/json/path_parser.go
      48 ./lib/json/path_lexer.go
      55 ./lib/json/query_lexer.go
     180 ./lib/excmd/argument_scanner.go
     171 ./lib/excmd/args_splitter.go
      13 ./lib/excmd/const.go
      74 ./lib/syntax/store.go
     377 ./lib/syntax/element.go
    2916 ./lib/syntax/syntax.go
     216 ./lib/query/terminal.go
     934 ./lib/query/query.go
     248 ./lib/query/header.go
     365 ./lib/query/encode.go
    1230 ./lib/query/error.go
      45 ./lib/query/alias.go
     164 ./lib/query/terminal_readline.go
     178 ./lib/query/aggregate_function.go
      59 ./lib/query/arithmetic.go
     307 ./lib/query/join.go
     596 ./lib/query/procedure.go
      71 ./lib/query/io.go
      58 ./lib/query/runtime_information.go
      27 ./lib/query/cell.go
    1321 ./lib/query/filter.go
      87 ./lib/query/record.go
     264 ./lib/query/string_formatter.go
     263 ./lib/query/user_defined_function.go
     154 ./lib/query/utils.go
    2384 ./lib/query/completer_readline.go
     242 ./lib/query/sort_value.go
    1845 ./lib/query/view.go
     183 ./lib/query/variable.go
     165 ./lib/query/comparison.go
     121 ./lib/query/uncommitted_view_map.go
    1118 ./lib/query/built_in_command.go
     155 ./lib/query/goroutine_manager.go
    1586 ./lib/query/function.go
     367 ./lib/query/cursor.go
     405 ./lib/query/file_info.go
     254 ./lib/query/object_writer.go
     785 ./lib/query/analytic_function.go
     134 ./lib/query/terminal_general.go
      73 ./lib/query/inline_tables.go
     215 ./lib/query/view_map.go
      48 ./help.go
     461 ./main.go
   32902 total
```
