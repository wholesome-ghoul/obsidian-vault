## misc

## go cli
 ```bash
# view coverage as html
go tool cover -html="coverage.out"
go tool cover -html="coverage.out" -o coverage.html
# check cpu prof
go tool pprof cpu.pprof

# check if what flags are enabled
go version -m <binary>

# generate benchmark
go test -bench=. -run=xxx -benchmem -memprofile mem.pprof -cpuprofile cpu.pprof -benchtime=10s > 0.bench
# generate coverage
go test -coverprofile coverage.out
go test -cover
go test -race

# compare benchmarks
# go install golang.org/x/perf/cmd/benchstat@latest
benchstat 0.bench 1.bench

# go install golang.org/x/tools/cmd/benchcmp
benchcmp 0.bench 1.bench

# get doc
go doc sync.WaitGroup
godoc -http=:6060

# in the root
go work init
go work use <name-of-module>

go install github.com/kisielk/errcheck/errcheck

# profile guided optimization (pgo)
curl -o cpu.pprof "http://localhost:8080/debug/pprof/profile?seconds=30"
mv cpu.prof default.pgo

# watcher
go install github.com/githubnemo/CompileDaemon@latest
CompileDaemon --build="make build" --directory=. --recursive=true -pattern="(.+\.go|.+\.env)" -exclude=whatever -exclude=.env --command=./name
```
