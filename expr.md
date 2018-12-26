# Repository
- Expr is an engine that can evaluate expressions.
- https://github.com/antonmedv/expr

# Eval
```
// eval.go
// Eval parses and evaluates given input.
func Eval(input string, env interface{}) (interface{}, error) {
	node, err := Parse(input)
	if err != nil {
		return nil, err
	}
	return Run(node, env)
}
```
# Parser

# Runner
```
// Run evaluates given ast.
func Run(node Node, env interface{}) (out interface{}, err error) {
	defer func() {
		if r := recover(); r != nil {
			err = fmt.Errorf("%v", r)
		}
	}()

	return node.Eval(env)
}
```

# Example1
```
env := map[string]interface{}{
    "foo": 1,
    "bar": struct{Value int}{1},
}

out, err := expr.Eval("foo + bar.Value", env)
```

# Example2
# Call function in expr.
```
type env struct {
	Name string
}

func (e env) Title() string {
	return strings.Title(e.Name)
}

p, err := expr.Parse("'Hello ' ~ Title()", expr.Env(env{}))

out, err := expr.Run(p, env{"world"})
```

# Example3
# Call function in expr.
# Embed struct as a object??
```
type env struct {
	helpers
	Name string
}

type helpers struct{}

func (h helpers) Title(s string) string {
	return strings.Title(s)
}

p, err := expr.Parse("'Hello ' ~ Title(Name)", expr.Env(env{}))

out, err := expr.Run(p, env{"world"})
```

# Sources
```
tree -h
.
├── [1.0K]  LICENSE
├── [3.3K]  README.md
├── [ 564]  UPGRADE.md
├── [1.6K]  bench_test.go
├── [3.6K]  doc.go
├── [3.5K]  doc_test.go
├── [ 375]  error.go
├── [7.4K]  eval.go
├── [ 10K]  eval_test.go
├── [6.1K]  lexer.go
├── [2.8K]  lexer_test.go
├── [1.1K]  node.go
├── [ 13K]  parser.go
├── [4.4K]  parser_test.go
├── [3.0K]  print.go
├── [2.9K]  print_test.go
├── [3.9K]  runtime.go
├── [ 11K]  type.go
└── [6.4K]  type_test.go

0 directories, 19 files
```
