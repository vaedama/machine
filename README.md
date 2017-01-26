# Memcached Server
A binary protocol implementation of the common memcached server. Only get, set, cas and delete are supported.

## Dependencies
- Standard Scala Library (2.12.1)
- Akka (2.4.16) for TCP server
- Logback Classic for logging
- Test dependencies: net.spy.spymemcached (2.12.1) and ScalaTest (3.0.1)

## Usage Instructions
- clone the project
- install `sbt` (http://www.scala-sbt.org/1.0/docs/Setup.html)
- `cd` to the project directory and run `sbt run` to start the memcached server (`sbt ~re-start` for dev mode). The default port number is 11211 (can be configured from src/main/resources/application.conf)
- On a different terminal window, try out the following commands:

```
telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.

set key 0 0 5 value
STORED
END

get key
VALUE key 0 5
value
END

get unknownkey
END

cas unknownkey 0 0 5 1 value
NOT_FOUND
END

set unknownkey 0 0 5 value
STORED
END

gets unknownkey
VALUE unknownkey 0 5 2
value
END

cas unknownkey 0 0 5 3 value
EXISTS
END

cas unknownkey 0 0 5 2 value2
STORED
END

get unknownkey
VALUE unknownkey 0 6
value2
END
```

## TODOs
- Support for flags.
- Support for no-reply. This is perhaps simple to implement.
- Support for expiry-fields.
- Improve logging.
- Add more tests, in general (esp. serialization and deserialization of complex objects).
- Add accuracy tests to stress under load.
- Add benchmarks to tune resource consumption (https://github.com/twitter/twemperf ???)
- Investigate other executors (see src/main/resources/application.conf)
- Investigate storing objects in other forms (JSON, protobuf etc.)

## Suggested improvements
- Fix known limitations
- Add examples with a real database
- Implement support for distributed cache mechanism (Akka cluster + consistent hashing ???)
- Integrate statistics (https://github.com/benschwarz/amnesia ???)
- Support missing commands
- Support UDP interface
- Build a mechanism to handle abusive clients
- Support text protocol for interested clients

## Testing
- `cd` to project directory
- run `sbt test` command
