# Zenoh Python Examples

## Get Started

```bash
python3 <example.py>
```

Each example accepts the `-h` or `--help` option that provides a description of its arguments and their default values.

If you run the tests against the zenoh router running in a Docker container, you need to add the
`-e tcp/localhost:7447` option to your examples. That's because Docker doesn't support UDP multicast
transport, and therefore the zenoh scouting and discrovery mechanism cannot work with.

## Examples description

### z_scout

Scouts for zenoh peers and routers available on the network.

Typical usage:

   ```bash
      python3 z_scout.py
   ```

### z_info

Gets information about the Zenoh session.

Typical usage:

   ```bash
      python3 z_info.py
   ```

### z_put

Puts a path/payload into Zenoh.
The path/payload will be received by all matching subscribers, for instance the [z_sub](#z_sub)
and [z_storage](#z_storage) examples.

Typical usage:

   ```bash
      python3 z_put.py
   ```

or

   ```bash
      python3 z_put.py -k demo/example/test -p 'Hello World'
   ```

### z_pub

Declares a resource with a path and a publisher on this resource. Then puts a payload using the numerical resource id.
The path/payload will be received by all matching subscribers, for instance the [z_sub](#z_sub)
and [z_storage](#z_storage) examples.

Typical usage:

   ```bash
      python3 z_pub.py
   ```

or

   ```bash
      python3 z_pub.py -k demo/example/test -p 'Hello World'
   ```

### z_sub

Creates a subscriber with a key expression.
The subscriber will be notified of each put made on any key expression matching
the subscriber's key expression, and will print this notification.

Typical usage:

   ```bash
      python3 z_sub.py
   ```

or

   ```bash
      python3 z_sub.py -k 'demo/**'
   ```

### z_get

Sends a query message for a selector.
The queryables with a matching path or selector (for instance [z_queryable](#z_queryable) and [z_storage](#z_storage))
will receive this query and reply with paths/payloads that will be received by the query callback.

Typical usage:

   ```bash
      python3 z_get.py
   ```

or

   ```bash
      python3 z_get.py -s 'demo/**'
   ```

### z_querier

Continuously sends query messages for a selector.
The queryables with a matching path or selector (for instance [z_queryable](#z_queryable) and [z_storage](#z_storage))
will receive these queries and reply with paths/payloads that will be received by the querier's query callback.

Typical usage:

   ```bash
      python3 z_querier.py
   ```

or

   ```bash
      python3 z_get.py -s 'demo/**'
   ```

### z_queryable

Creates a queryable function with a key expression.
This queryable function will be triggered by each call to a get operation on zenoh
with a selector that matches the key expression, and will return a payload to the querier.

Typical usage:

   ```bash
      python3 z_queryable.py
   ```

or

   ```bash
      python3 z_queryable.py -k demo/example/queryable -p 'This is the result'
   ```

### z_storage

Trivial implementation of a storage in memory.
This examples creates a subscriber and a queryable on the same key expression.
The subscriber callback will store the received key/values in an hashmap.
The queryable callback will answer to queries with the key/values stored in the hashmap
and that match the queried selector.

Typical usage:

   ```bash
      python3 z_storage.py
   ```

or

   ```bash
      python3 z_storage.py -k 'demo/**'
   ```

### z_pub_thr & z_sub_thr

Pub/Sub throughput test.
This example allows to perform throughput measurements between a pubisher performing
put operations and a subscriber receiving notifications of those puts.

Typical Subscriber usage:

   ```bash
      python3 z_sub_thr.py
   ```

Typical Publisher usage:

   ```bash
      python3 z_pub_thr.py 1024
   ```
   
### z_advanced_pub

Publishes data using the Advanced Publisher feature of Zenoh, which adds reliability mechanisms like sample caching, miss detection, and publisher discovery.

Typical usage:

```bash
   python3 z_advanced_pub.py
```

or

```bash
   python3 z_advanced_pub.py -k demo/example/zenoh-python-pub -p 'Hello World' --history 10
```

### z_advanced_sub

Subscribes using the Advanced Subscriber feature of Zenoh. Supports late publisher detection, recovery, and detection of missed samples.

Typical usage:

```bash
   python3 z_advanced_sub.py
```

or

```bash
   python3 z_advanced_sub.py -k demo/example/**
```

### z_bytes

Demonstrates how to handle raw bytes, strings, JSON, protobuf, and Python objects using `ZBytes` and `z_serialize`/`z_deserialize`.

Typical usage:

```bash
   python3 z_bytes.py
```

### z_delete

Deletes resources in Zenoh matching the given key expression.

Typical usage:

```bash
   python3 z_delete.py
```

or

```bash
   python3 z_delete.py -k demo/example/zenoh-python-put
```

### z_get_liveliness

Queries all currently alive liveliness tokens that match a given key expression.

Typical usage:

```bash
   python3 z_get_liveliness.py
```

or

```bash
   python3 z_get_liveliness.py -k group1/** -o 5.0
```

### z_liveliness

Declares a liveliness token on a given key. The token stays alive while the process is running.

Typical usage:

```bash
   python3 z_liveliness.py
```

or

```bash
   python3 z_liveliness.py -k group1/zenoh-py
```

### z_ping

Measures round-trip latency between ping and pong using Zenoh pub/sub. Sends data and waits for response.

Typical usage:

```bash
   python3 z_ping.py 1024
```

Optional:

```bash
   python3 z_ping.py --warmup 1.0 --samples 100 1024
```

### z_pong

Receives data from z_ping and replies back, completing the round-trip latency loop.

Typical usage:

```bash
   python3 z_pong.py
```

or

```bash
   python3 z_pong.py --express True
```

### z_pull

Implements a pull-based subscriber using a ring buffer to retrieve recent samples.

Typical usage:

```bash
   python3 z_pull.py
```

or

```bash
   python3 z_pull.py -k demo/example/** --size 5 --interval 2
```

### z_put_float

Publishes a floating-point number using Zenoh's typed serialization.

Typical usage:

```bash
   python3 z_put_float.py
```

or

```bash
   python3 z_put_float.py -k demo/example/zenoh-python-put -p 3.1415
```

### z_sub_liveliness

Subscribes to changes in liveliness tokens (alive or dropped) over a given key expression.

Typical usage:

```bash
   python3 z_sub_liveliness.py
```

or

```bash
   python3 z_sub_liveliness.py -k group1/** --history True
```

### z_sub_queued

Receives samples with a queued subscriber and prints notifications for each message.

Typical usage:

```bash
   python3 z_sub_queued.py
```

or

```bash
   python3 z_sub_queued.py -k demo/example/**
```
