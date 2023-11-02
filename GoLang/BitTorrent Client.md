---
tags: go, bittorrent
deck: go
---

# What is metainfo file? #card
<!-- 1698849877293 b11fed0365d110a5d1c9187b17269e1d -->

Basically, a `.torrent` file containing all the necessary information to start a connection.

Contains bencoded dictionary of `announce` and `info`. Where `announce` is a URL of a tracker and `info` is a bencoded dictionary of: name, piece length, pieces, length

# How to decode metainfo file? #card
<!-- 1698850313131 25d86f12c4a1f7f9f5071a14489f16e9 -->

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
<!-- 1698850657230 b78622a26a80cb19e5a913c176124507 -->

```go
var buf bytes.Buffer
err := bencode.Marshal(&byf, someData)
hash := sha1.Sum(buf.Bytes())
```

# Boolean flag example in Golang test #card
<!-- 1698882536483 0301a5bed4d967517bd2cbdc2c29e98d -->

```go
var update = flag.Bool("update", false, "update some files")

func TestFoo(t *testing.T) {
  if *update {
    // ...
  }
}
```
