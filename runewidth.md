# Repository
- wcwidth for golang
- https://github.com/mattn/go-runewidth

# Example1
```go
runewidth.StringWidth("つのだ☆HIRO") == 12
```

# source path 1
```go
// runewidth.go
// StringWidth return width as you can see
func StringWidth(s string) (width int) {
	return DefaultCondition.StringWidth(s)
}

// DefaultCondition is a condition in current locale
var DefaultCondition = &Condition{EastAsianWidth}

```

# source path 2
```go
// Condition have flag EastAsianWidth whether the current locale is CJK or not.
var (
	// EastAsianWidth will be set true if the current locale is CJK
	EastAsianWidth bool

	// DefaultCondition is a condition in current locale
	DefaultCondition = &Condition{EastAsianWidth}
)

func init() {
	env := os.Getenv("RUNEWIDTH_EASTASIAN")
	if env == "" {
		EastAsianWidth = IsEastAsian()
	} else {
		EastAsianWidth = env == "1"
	}
}

// func IsEastAsian() bool is defined in
//   runewidth_js.go
//   runewidth_posix.go
//   runewidth_windows.go

```

# source path 3
```go
// StringWidth return width as you can see
func (c *Condition) StringWidth(s string) (width int) {
	for _, r := range []rune(s) {
		width += c.RuneWidth(r)
	}
	return width
}

// RuneWidth returns the number of cells in r.
// See http://www.unicode.org/reports/tr11/
func (c *Condition) RuneWidth(r rune) int {
	switch {
	case r < 0 || r > 0x10FFFF ||
		inTables(r, nonprint, combining, notassigned):
		return 0
	case (c.EastAsianWidth && IsAmbiguousWidth(r)) ||
		inTables(r, doublewidth, emoji):
		return 2
	default:
		return 1
	}
}

func inTables(r rune, ts ...table) bool {
	for _, t := range ts {
		if inTable(r, t) {
			return true
		}
	}
	return false
}

func inTable(r rune, t table) bool {
	// func (t table) IncludesRune(r rune) bool {
	if r < t[0].first {
		return false
	}

	bot := 0
	top := len(t) - 1
	for top >= bot {
		mid := (bot + top) / 2

		switch {
		case t[mid].last < r:
			bot = mid + 1
		case t[mid].first > r:
			top = mid - 1
		default:
			return true
		}
	}

	return false
}


```

# Sources
```
tree -h
.
├── [1.1K]  LICENSE
├── [ 806]  README.mkd
├── [ 60K]  runewidth.go
├── [ 169]  runewidth_js.go
├── [1.3K]  runewidth_posix.go
├── [1.4K]  runewidth_posix_test.go
├── [6.6K]  runewidth_test.go
└── [ 416]  runewidth_windows.go

0 directories, 8 files
```
