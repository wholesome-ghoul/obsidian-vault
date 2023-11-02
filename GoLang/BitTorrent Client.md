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

# Boolean flag example in go test #card
<!-- 1698882536483 a83a1ddcb55e6bc623da92e3f4f34457 -->

```go
var update = flag.Bool("update", false, "update some files")

func TestFoo(t *testing.T) {
  if *update {
    // ...
  }
}
```

# Build uri for GET request #card
<!-- 1698885891488 f24a4c8dd972ca16cd99b9bca5bec9e3 -->

```go
import "net/url"

func Foo(rawUrl string) string {
  url, err := url.Parse(rawUrl)
  // ...

  params := url.Values{
    "param1": []string{"0"},
    "param2": []string{string(someValue)},
  }
  url.RawQuery = params.Encode()

  return url.String()
}
```

# Write peers parser (Bittorrent client)? #card
<!-- 1698936230498 f2a2989b6a655a2a785e59fd12a878e2 -->

```go
import ( 
  "encoding/binary"
  "net"
)

// compact version
const PeerSize = 6

type Peer {
  IP net.IP
  Port uint16
}

func Parse(rawPeers string) []Peer {
  numOfPeers := len(rawPeers) / PeerSize
  peersBin := []byte(rawPeers)

  peers := make([]Peer, numOfPeers)
  for i := 0; i < numOfPeers; i++ {
    offset := i * PeerSize
    peers[i].IP = peersBin[offset:offset+4]
    peers[i].Port = binary.BigEndian.Uint16(peersBin[offset+4:offset+6])
  }

  return peers
}
```

# Byte sizes of types in go #card
<!-- 1698936230544 6e62dab6c00c6fd2d49f09a9f013bcf6 -->

```
bool, int8/uint8 - 1 byte
int16, uint16 - 2 bytes
int32, uint32, float32 - 4 bytes
int64, uint64, float64, pointer - 8 bytes
string - 16 bytes (2 alignments of 8 bytes)
any slice takes 24 bytes (3 alignments of 8 bytes)
array of length n takes n*type of bytes
```
