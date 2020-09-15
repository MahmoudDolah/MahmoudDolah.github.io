---
layout: post
title:  "How to Load Test a GraphQL Application using Apache Bench"
date:   2020-09-15 06:39:32 -0400

categories: DevOps graphql apachebench
---

This week, I've been given the important task of finding the bottleneck in a new graphql application that will be launching soon. Load testing isn't something [new to me][magento-loadtest], so I got to work. I started firing up requests using Apache Bench, but was unable to get a single request to not return a 5xx error.

```
$ ab -n 10 -c 1 -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept: application/json' -H 'Origin: https://REDACTED' 'https://REDACTED/graphql'
This is ApacheBench, Version 2.3 <$Revision: 1757674 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
Benchmarking REDACTED (be patient).....done
Server Software:
Server Hostname:        REDACTED
Server Port:            443
SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES128-GCM-SHA256,2048,128
TLS Server Name:        REDACTED
Document Path:          /graphql
Document Length:        18 bytes
Concurrency Level:      1
Time taken for tests:   0.156 seconds
Complete requests:      10
Failed requests:        0
Non-2xx responses:      10			<-- All the requests failed
Total transferred:      2590 bytes
HTML transferred:       180 bytes
Requests per second:    63.93 [#/sec] (mean)
Time per request:       15.642 [ms] (mean)
Time per request:       15.642 [ms] (mean, across all concurrent requests)
Transfer rate:          16.17 [Kbytes/sec] received
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        5    6   0.3      6       6
Processing:     5   10   9.4      7      36
Waiting:        5   10   9.4      7      36
Total:         11   16   9.5     14      42
Percentage of the requests served within a certain time (ms)
  50%     14
  66%     14
  75%     14
  80%     17
  90%     42
  95%     42
  98%     42
  99%     42
 100%     42 (longest request)
```

***Note: You can tell all the requests failed via the line `Non-2xx responses: 10`***

I was able to properly `curl` the service, but was unable to hit it with postman or `ab`.
```
$ curl 'https://<REDACTED>/graphql' -H 'Accept-Encoding: gzip, deflate, br' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Connection: keep-alive' -H 'DNT: 1' -H 'Origin: REDACTED' --data-binary '{"query":"{getImage(data: {id: \"REDACTED\"}){\n  image{createdAt, source, link, id, width, md5}\n}} "}' --compressed
{"data":{"getImage":{"image":{"createdAt":"1304972624","source":null,"link":null,"id":"REDACTED","width":900,"md5":"REDACTED"}}}}
```

Taking a look again, I realized that I was incorrectly incorrectly passing the graphql query. Instead of passing it as a header, you need to pass it using the `-p` flag.

```
$ cat post.txt
{"query":"GRAPHQL QUERY"}
$ ab -n 2000 -c 100 -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept: application/json' -H 'Origin: https://REDACTED' -T 'application/json' -p post.txt 'https://REDACTED/graphql'
```

[magento-loadtest]: https://www.dolah.dev/devops/magento/2018/10/02/magento-load-test.html
