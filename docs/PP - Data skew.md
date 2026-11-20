Data skew. In real systems, data or load is usually distributed unevenly:
- **storage skew** — some keys or clients hold far more data than others;
- **traffic skew** — some keys or clients receive far more requests than others.

## Methods for handling skew

TODO — methods will be added over time.

## Hot key / hot partition

When a database is split into several separate databases, or partitioned by a specific key within a single database (partition key). This way we spread the load across several partitions.

Hot key — a key that gets a disproportionately large share of data or requests; its partition is overloaded while the others sit idle.

Partitioning brings its own problems and patterns for solving them.

### Celebrity problem

A special case of skew where one client is too large either in load or in data volume.

Popular example: a celebrity has only a few posts, but millions of people read them. Normally a post is pushed into followers' feeds at write time. I publish a post — it immediately appears in the feeds of my 100 followers. When they open the app, their feed is already precomputed. When a celebrity publishes a post, it has to appear in millions of feeds, and the fan-out queue gets clogged. That's why, for celebrities, posts are added to the feed when the follower reads it, not when the celebrity writes.

![Celebrity problem: hybrid fan-out (push + pull)](../files/PP%20-%20Data%20skew%20-%20Celebrity%20hybrid%20fan-out.png)

Diagram: [Excalidraw (online)](https://app.excalidraw.com/s/4fuBV9gHb6h/9kYcBEFeZfv) · [.excalidraw file](../files/PP%20-%20Data%20skew%20-%20Celebrity%20hybrid%20fan-out.excalidraw)

Celebrity by volume example: a client with 10,000× more data than others — then it makes sense to move this client to a dedicated shard.
