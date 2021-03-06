# bombardier
bombardier is a HTTP(S) benchmarking tool. It is written in Go programming language and uses excellent [fasthttp](https://github.com/valyala/fasthttp) instead of Go's default http library, because of its lightning fast performance.

##Installation
You can grab the latest version in the [releases](https://github.com/codesenberg/bombardier/releases) section.
Alternatively, just run `go get github.com/codesenberg/bombardier`.

##Usage
Run it like:
```
bombardier <options> <url>
```
Also, you can supply these options:
```
  -H value
        HTTP headers to use
  -c uint
        Maximum number of concurrent connections (default 200)
  -n value
        Number of requests
  -d value
        Duration of test
  -data string
        Request body
  -latencies
        Print latency statistics
  -m string
        Request method (default "GET")
  -timeout duration
        Socket/request timeout (default 2s)
```
To set multiple headers just repeat the H flag, like so:
```
bombardier -H 'First: Value1' -H 'Second: Value2' -H 'Third: Value3' http://somehost:8080
```
You should see something like this if you done everything correctly:
```
> bombardier -c 125 -n 10000000 http://localhost:8080
Bombarding http://localhost:8080 with 10000000 requests using 125 connections
10000000 / 10000000 [============================================] 100.00 % 40s Done!
Statistics        Avg      Stdev        Max
  Reqs/sec    246782.00   11798.53     257026
  Latency      505.00us   516.77us    51.00ms
  HTTP codes:
    1xx - 0, 2xx - 10000000, 3xx - 0, 4xx - 0, 5xx - 0
    others - 0
  Throughput:   273.22MB/s
```
Or, on a realworld server(with latency distribution):
```
> bombardier -c 200 -d 10s --latencies http://google.com
Bombarding http://google.com for 10s using 200 connections
[==========================================================================]10s Done!
Statistics        Avg      Stdev        Max
  Reqs/sec      5384.00     789.97       5699
  Latency       36.96ms    19.58ms      1.44s
  Latency Distribution
     50%    34.00ms
     75%    41.00ms
     90%    42.00ms
     99%    45.00ms
  HTTP codes:
    1xx - 0, 2xx - 0, 3xx - 54083, 4xx - 0, 5xx - 0
    errored - 2
  Throughput:     2.51MB/s
```
