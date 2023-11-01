---
tags: go, bittorrent
deck: go
---

# What is metainfo file? #card
<!-- 1698849877293 8b0094ad65f81dd21eb275ccffb46372 -->

Basically, a `.torrent` file containing all the necessary information to start a connection.

# How to decode metainfo file? #card
<!-- 1698850313131 2cbde93f91774a178426310989bc7cf8 -->

```go
type Metainfo struct {
  Announce    string
  InfoHash    [20]byte
  Name        string
  PieceLength int
  Pieces      [][20]byte
  Length      int
}

type bencodedMetainfoFile struct {
  Announce string       `bencode:"announce"`
  Info     bencodedInfo `bencode:"info"`
}

type bencodedInfo struct {
  Name        string `bencode:"name"`
  PieceLength int    `bencode:"piece length"`
  Pieces      string `bencode:"pieces"`
  Length      int    `bencode:"length"`
}

// opening metainfo file
func Open(path string) (Metainfo, error) {
  file, err := os.Open(file)
  if err != nil {
    // ...
  }
  defer file.Close()

  buf := bencodedMetainfoFile{}
  err = bencode.Unmarshal(file, &buf)
  if err != nil {
    // ...
  }

  ...
}
```

# How to calculate sha1 sum? #card
<!-- 1698850657230 44e1f2cadb8eefcbe764a59f019848ad -->

```go
var buf bytes.Buffer
err := bencode.Marshal(&byf, someData)
hash := sha1.Sum(buf.Bytes())
```
