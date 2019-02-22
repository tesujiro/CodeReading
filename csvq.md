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
```
