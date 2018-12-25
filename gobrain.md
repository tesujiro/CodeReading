# Repository
- Neural Networks written in go
- https://github.com/goml/gobrain

# Example1
```go
func main() {
	// set the random seed to 0
	rand.Seed(0)

	// create the XOR representation patter to train the network
	patterns := [][][]float64{
		{{0, 0}, {0}},
		{{0, 1}, {1}},
		{{1, 0}, {1}},
		{{1, 1}, {0}},
	}

	// instantiate the Feed Forward
	ff := &gobrain.FeedForward{}

	// 2 inputs, 2 hidden nodes and 1 output.
	ff.Init(2, 2, 1)

	// train the network using the XOR patterns
	// the training will run for 1000 epochs
	// the learning rate is set to 0.6 and the momentum factor to 0.4
	// use true in the last parameter to receive reports about the learning error
	ff.Train(patterns, 1000, 0.6, 0.4, true)
}
```

# source path 1
```go
```

# Sources
```
tree -h
.
├── [1.1K]  LICENSE
├── [2.6K]  README.md
├── [5.6K]  feedforward.go
├── [1.1K]  feedforward_test.go
└── [ 506]  util.go

0 directories, 5 files

```
