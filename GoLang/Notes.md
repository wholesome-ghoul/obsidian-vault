```go
filepath.Clean(path)
// does not follow sym links
// less efficient than WalkDir
filepath.Walk(path, fn)
filepath.Base(path)
filepath.Dir(path)
filepath.Ext(path)

os.Lstat(path)
os.Stat(path)

// is just string(b1) == string(b2)
bytes.Equal(b1, b2)

// create a temporary file (max retries 10,000)
os.CreateTemp(dir, pattern)
// removes a file or an empty dir
os.Remove(name)
os.Chmod(name, mode)
os.Rename(name, newName)
os.MkdirAll(path, perm)

// get file (*os.File) name
f.Name()
f.Write(bytes)
f.Close()
```