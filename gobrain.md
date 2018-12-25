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

# Data Structure : FeedForward
```
// FeedForwad struct is used to represent a simple neural network
type FeedForward struct {
	// Number of input, hidden and output nodes
	NInputs, NHiddens, NOutputs int
	// Whether it is regression or not
	Regression bool
	// Activations for nodes
	InputActivations, HiddenActivations, OutputActivations []float64
	// ElmanRNN contexts
	Contexts [][]float64
	// Weights
	InputWeights, OutputWeights [][]float64
	// Last change in weights for momentum
	InputChanges, OutputChanges [][]float64
}
```

# Init
```
	for i := 0; i < nn.NInputs; i++ {
		for j := 0; j < nn.NHiddens; j++ {
			nn.InputWeights[i][j] = random(-1, 1)
		}
	}

	for i := 0; i < nn.NHiddens; i++ {
		for j := 0; j < nn.NOutputs; j++ {
			nn.OutputWeights[i][j] = random(-1, 1)
		}

```
# Train
```go
/*
This method is used to train the Network, it will run the training operation for 'iterations' times
and return the computed errors when training.
*/
func (nn *FeedForward) Train(patterns [][][]float64, iterations int, lRate, mFactor float64, debug bool) []float64 {
	errors := make([]float64, iterations)

	for i := 0; i < iterations; i++ {
		var e float64
		for _, p := range patterns {
			nn.Update(p[0])

			tmp := nn.BackPropagate(p[1], lRate, mFactor)
			e += tmp
		}

		errors[i] = e

		if debug && i%1000 == 0 {
			fmt.Println(i, e)
		}
	}

	return errors
}

```

# Update
```
/*
The Update method is used to activate the Neural Network.

Given an array of inputs, it returns an array, of length equivalent of number of outputs, with values ranging from 0 to 1.
*/
func (nn *FeedForward) Update(inputs []float64) []float64 {
	if len(inputs) != nn.NInputs-1 {
		log.Fatal("Error: wrong number of inputs")
	}

	for i := 0; i < nn.NInputs-1; i++ {
		nn.InputActivations[i] = inputs[i]
	}

	for i := 0; i < nn.NHiddens-1; i++ {
		var sum float64

		for j := 0; j < nn.NInputs; j++ {
			sum += nn.InputActivations[j] * nn.InputWeights[j][i]
		}

		// compute contexts sum
		for k := 0; k < len(nn.Contexts); k++ {
			for j := 0; j < nn.NHiddens-1; j++ {
				sum += nn.Contexts[k][j]
			}
		}

		nn.HiddenActivations[i] = sigmoid(sum)
	}
```

# BackPropagate
```
/*
The BackPropagate method is used, when training the Neural Network,
to back propagate the errors from network activation.
*/
func (nn *FeedForward) BackPropagate(targets []float64, lRate, mFactor float64) float64 {
	if len(targets) != nn.NOutputs {
		log.Fatal("Error: wrong number of target values")
	}

	outputDeltas := vector(nn.NOutputs, 0.0)
	for i := 0; i < nn.NOutputs; i++ {
		outputDeltas[i] = dsigmoid(nn.OutputActivations[i]) * (targets[i] - nn.OutputActivations[i])
	}

	hiddenDeltas := vector(nn.NHiddens, 0.0)
	for i := 0; i < nn.NHiddens; i++ {
		var e float64

		for j := 0; j < nn.NOutputs; j++ {
			e += outputDeltas[j] * nn.OutputWeights[i][j]
		}

		hiddenDeltas[i] = dsigmoid(nn.HiddenActivations[i]) * e
	}

	for i := 0; i < nn.NHiddens; i++ {
		for j := 0; j < nn.NOutputs; j++ {
			change := outputDeltas[j] * nn.HiddenActivations[i]
			nn.OutputWeights[i][j] = nn.OutputWeights[i][j] + lRate*change + mFactor*nn.OutputChanges[i][j]
			nn.OutputChanges[i][j] = change
		}
	}

	for i := 0; i < nn.NInputs; i++ {
		for j := 0; j < nn.NHiddens; j++ {
			change := hiddenDeltas[j] * nn.InputActivations[i]
			nn.InputWeights[i][j] = nn.InputWeights[i][j] + lRate*change + mFactor*nn.InputChanges[i][j]
			nn.InputChanges[i][j] = change
		}
	}

	var e float64

	for i := 0; i < len(targets); i++ {
		e += 0.5 * math.Pow(targets[i]-nn.OutputActivations[i], 2)
	}

	return e
}
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
