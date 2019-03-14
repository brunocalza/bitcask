# bitcask

[![Build Status](https://cloud.drone.io/api/badges/prologic/bitcask/status.svg)](https://cloud.drone.io/prologic/bitcask)
[![CodeCov](https://codecov.io/gh/prologic/bitcask/branch/master/graph/badge.svg)](https://codecov.io/gh/prologic/bitcask)
[![Go Report Card](https://goreportcard.com/badge/prologic/bitcask)](https://goreportcard.com/report/prologic/bitcask)
[![GoDoc](https://godoc.org/github.com/prologic/bitcask?status.svg)](https://godoc.org/github.com/prologic/bitcask) 
[![Sourcegraph](https://sourcegraph.com/github.com/prologic/bitcask/-/badge.svg)](https://sourcegraph.com/github.com/prologic/bitcask?badge)

A Bitcask (LSM+WAL) Key/Value Store written in Go.

## Features

* Embeddable
* Builtin CLI
* Builtin Redis-compatible server
* Predictable read/write performance
* Low latecny
* High throughput (See: [Performance](README.md#Performance)

## Install

```#!bash
$ go get github.com/prologic/bitcask
```

## Usage (library)

Install the package into your project:

```#!bash
$ go get github.com/prologic/bitcask
```

```#!go
package main

import (
    "log"

    "github.com/prologic/bitcask"
)

func main() {
    db, _ := bitcask.Open("/tmp/db")
    db.Set("Hello", []byte("World"))
    db.Close()
}
```

See the [godoc](https://godoc.org/github.com/prologic/bitcask) for further
documentation and other examples.

## Usage (tool)

```#!bash
$ bitcask -p /tmp/db set Hello World
$ bitcask -p /tmp/db get Hello
World
```

## Usage (server)

There is also a builtin very  simple Redis-compatible server called `bitcaskd`:

```#!bash
$ ./bitcaskd ./tmp
INFO[0000] starting bitcaskd v0.0.7@146f777              bind=":6379" path=./tmp
```

Example session:

```
$ telnet localhost 6379
Trying ::1...
Connected to localhost.
Escape character is '^]'.
SET foo bar
+OK
GET foo
$3
bar
DEL foo
:1
GET foo
$-1
PING
+PONG
QUIT
+OK
Connection closed by foreign host.
```

## Performance

Benchmarks run on a 11" Macbook with a 1.4Ghz Intel Core i7:

```
$ make bench
...
BenchmarkGet/128B-4         	  200000	      5780 ns/op	     400 B/op	       5 allocs/op
BenchmarkGet/256B-4         	  200000	      6138 ns/op	     656 B/op	       5 allocs/op
BenchmarkGet/512B-4         	  200000	      5967 ns/op	    1200 B/op	       5 allocs/op
BenchmarkGet/1K-4           	  200000	      6290 ns/op	    2288 B/op	       5 allocs/op
BenchmarkGet/2K-4           	  200000	      6293 ns/op	    4464 B/op	       5 allocs/op
BenchmarkGet/4K-4           	  200000	      7673 ns/op	    9072 B/op	       5 allocs/op
BenchmarkGet/8K-4           	  200000	     10373 ns/op	   17776 B/op	       5 allocs/op
BenchmarkGet/16K-4          	  100000	     14227 ns/op	   34928 B/op	       5 allocs/op
BenchmarkGet/32K-4          	  100000	     25953 ns/op	   73840 B/op	       5 allocs/op
BenchmarkPut/128B-4         	  100000	     17353 ns/op	     680 B/op	       5 allocs/op
BenchmarkPut/256B-4         	  100000	     18620 ns/op	     808 B/op	       5 allocs/op
BenchmarkPut/512B-4         	  100000	     19068 ns/op	    1096 B/op	       5 allocs/op
BenchmarkPut/1K-4           	  100000	     23738 ns/op	    1673 B/op	       5 allocs/op
BenchmarkPut/2K-4           	   50000	     25118 ns/op	    2826 B/op	       5 allocs/op
BenchmarkPut/4K-4           	   50000	     44605 ns/op	    5389 B/op	       5 allocs/op
BenchmarkPut/8K-4           	   30000	     55237 ns/op	   10001 B/op	       5 allocs/op
BenchmarkPut/16K-4          	   20000	     78966 ns/op	   18972 B/op	       5 allocs/op
BenchmarkPut/32K-4          	   10000	    116253 ns/op	   41520 B/op	       5 allocs/op
```

For 128B values:

* ~180,000 reads/sec
*  ~60,000 writes/sec

The full benchmark above shows linear performance as you increase key/value sizes.

## License

bitcask is licensed under the [MIT License](https://github.com/prologic/bitcask/blob/master/LICENSE)
