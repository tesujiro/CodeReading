
# Repository
- An implementation of the FileSystem interface for tar files.
https://github.com/posener/tarfs

# FIleSystem interface
```go
// see https://github.com/kr/fs/blob/main/filesystem.go
// FileSystem defines the methods of an abstract filesystem.
type FileSystem interface {

	// ReadDir reads the directory named by dirname and returns a
	// list of directory entries.
	ReadDir(dirname string) ([]os.FileInfo, error)

	// Lstat returns a FileInfo describing the named file. If the file is a
	// symbolic link, the returned FileInfo describes the symbolic link. Lstat
	// makes no attempt to follow the link.
	Lstat(name string) (os.FileInfo, error)

	// Join joins any number of path elements into a single path, adding a
	// separator if necessary. The result is Cleaned; in particular, all
	// empty strings are ignored.
	//
	// The separator is FileSystem specific.
	Join(elem ...string) string
}
```
# Library Sample
```
func ExampleRead() {

	// use tarfs.NewFile to open tar.gz files,
	f, err := tarfs.NewFile("./root.tar.gz")
	if err != nil {
		panic(err)
	}
	defer f.Close()

	// open a file inside the file
	f.Open("a/b/c/d")

	// read the file content
	buf, err := ioutil.ReadAll(f)
	if err != nil {
		panic(err)
	}

	fmt.Println(string(buf))

	// Output: hello
}
```

# Basic code path 1 : NewFile


# Sources
```
tree -h
.
├── [9.9K]  LICENSE.txt
├── [ 851]  README.md
├── [ 320]  examples
│   ├── [  96]  a
│   │   └── [  96]  b
│   │       └── [ 128]  c
│   │           ├── [   6]  d
│   │           └── [   6]  e
│   ├── [ 149]  a.tar.gz
│   ├── [1.1K]  example_test.go
│   ├── [2.1K]  external_test.go
│   ├── [ 128]  one-file.tar.gz
│   ├── [ 10K]  root.tar
│   ├── [ 201]  root.tar.gz
│   └── [ 149]  two-files.tar.gz
├── [1.7K]  file.go
├── [ 892]  file_test.go
├── [2.7K]  filesystem.go
├── [2.6K]  filesystem_test.go
├── [ 325]  go.test.sh
└── [ 447]  node.go

