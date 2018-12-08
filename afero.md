# Repository
- A FileSystem Abstraction System for Go
- https://github.com/spf13/afero

# Example1
```go
func writeAndCheck(fs afero.Fs) {
	// create test files and directories
	fs.MkdirAll("./src/a", 0755)
	afero.WriteFile(fs, "./src/a/b", []byte("file b"), 0644)
	afero.WriteFile(fs, "./src/c", []byte("file c"), 0644)
	afero.WriteFile(fs, "./src/あああ", []byte("file あああ"), 0644)
	name := "./src/c"
	_, err := fs.Stat(name)
	if os.IsNotExist(err) {
		fmt.Printf("file \"%s\" does not exist.\n", name)
	}
	if b, err := afero.ReadFile(fs, name); err != nil {
		fmt.Printf("file \"%s\" read error :%v\n", err)
	} else {
		fmt.Printf("%v\n", string(b))
	}
	if err := fs.RemoveAll("./src"); err != nil {
		fmt.Printf("Remove All error :%v\n", err)
	}
}

func main() {
	appFS := afero.NewMemMapFs()
	writeAndCheck(appFS)
	osFS := afero.NewOsFs()
	writeAndCheck(osFS)
}
```
# FileSystem interface
``` go
type Fs interface {
    // Create creates a file in the filesystem, returning the file and an
    // error, if any happens.
    Create(name string) (File, error)

    // Mkdir creates a directory in the filesystem, return an error if any
    // happens.
    Mkdir(name string, perm os.FileMode) error

    // MkdirAll creates a directory path and all parents that does not exist
    // yet.
    MkdirAll(path string, perm os.FileMode) error

    // Open opens a file, returning it or an error, if any happens.
    Open(name string) (File, error)

    // OpenFile opens a file using the given flags and the given mode.
    OpenFile(name string, flag int, perm os.FileMode) (File, error)

    // Remove removes a file identified by name, returning an error, if any
    // happens.
    Remove(name string) error

    // RemoveAll removes a directory path and any children it contains. It
    // does not fail if the path does not exist (return nil).
    RemoveAll(path string) error

    // Rename renames a file.
    Rename(oldname, newname string) error

    // Stat returns a FileInfo describing the named file, or an error, if any
    // happens.
    Stat(name string) (os.FileInfo, error)

    // The name of this FileSystem
    Name() string

    //Chmod changes the mode of the named file to mode.
    Chmod(name string, mode os.FileMode) error

    //Chtimes changes the access and modification times of the named file
    Chtimes(name string, atime time.Time, mtime time.Time) error
}
```
# Sources
```
tree -h
.
├── [9.9K]  LICENSE.txt
├── [ 14K]  README.md
├── [3.2K]  afero.go
├── [ 16K]  afero_test.go
├── [ 297]  appveyor.yml
├── [4.9K]  basepath.go
├── [4.7K]  basepath_test.go
├── [6.7K]  cacheOnReadFs.go
├── [8.9K]  composite_test.go
├── [ 721]  const_bsds.go
├── [ 766]  const_win_unix.go
├── [6.7K]  copyOnWriteFs.go
├── [ 729]  copyOnWriteFs_test.go
├── [  30]  go.mod
├── [2.6K]  httpFs.go
├── [6.6K]  ioutil.go
├── [3.0K]  ioutil_test.go
├── [1.0K]  lstater.go
├── [3.0K]  lstater_test.go
├── [2.8K]  match.go
├── [4.2K]  match_test.go
├── [ 192]  mem
│   ├── [ 971]  dir.go
│   ├── [1.3K]  dirmap.go
│   ├── [6.7K]  file.go
│   └── [2.7K]  file_test.go
├── [7.3K]  memmap.go
├── [9.5K]  memmap_test.go
├── [2.6K]  os.go
├── [2.9K]  path.go
├── [1.6K]  path_test.go
├── [1.6K]  readonlyfs.go
├── [4.1K]  regexpfs.go
├── [2.1K]  ro_regexp_test.go
├── [ 160]  sftpfs
│   ├── [2.0K]  file.go
│   ├── [3.1K]  sftp.go
│   └── [7.2K]  sftp_test_go
├── [6.4K]  unionFile.go
├── [7.2K]  util.go
└── [ 13K]  util_test.go

2 directories, 39 files
```
