# nats-go-cheat

NATS Go SDK cheats (nats.go, 1.11+)

## "Core" notes

There are multiple options of publishing and subscribing to NATS for ephemeral messaging.

|  | Description | nats.go (Conn) |
| --- | --- | --- |
| Publish | Client publish  | Publish |
| PublishRequest | Client Request publish, non-blocking | PublishRequest |
| Request | Client Request publish blocking on return reply | Request |
| Subscribe | Client explicitly subscribed to a subject | Subscribe, SubscribeSync, QueueSubscribe, QueueSubscribeSync, ChanSubscribe, ChanQueueSubscribe, QueueSubscribeSyncWithChan |

Client Consumer subscribe options

| Option (Conn) | Desc | Interest scope | # Per-msg delivery pattern | 
| --- | --- | --- | --- |
| Subscribe | Callback handler function for each message received | subject | N clients |
| SubscribeSync | Can check .Pending and use .NextMsg | subject | N clients |
| QueueSubscribe | Same as Subscribe, but a "queue" group | subject + group | 1 of N clients per group|
| QueueSubscribeSync | Same as SubscribeSync, but a "queue" group | subject + group | 1 of N clients per group |
| ChanSubscribe | Channel for each message received | subject | N clients |
| ChanQueueSubscribe | Same as ChanSubscribe, but a "queue" group | subject + group | 1 of N clients per group |
| QueueSubscribeSyncWithChan | Same as ChanQueueSubscribe, but with .Pending and .NextMsg | subject + group | 1 of N clients per group |

## JetStream notes

There are multiple options of publishing and subscribing to JetStreams for durable messaging.  

|  | Description | nats.go (Conn.JetStream) |
| --- | --- | --- |
| Publish (Sync) | Client (req) with JS ACK (rply) | Publish |
| Publish (Async) | Client (pub) to JS (sub) with future JS ACK (rply) | PublishAsync |
| Subscribe (Pull) | Client (req) from JS Consumer (rply) | PullSubscribe |
| Subscribe (Push) | JS Consumer (pub) to Client (sub) | Subscribe, SubscribeSync, QueueSubscribe, QueueSubscribeSync, ChanSubscribe |

Client Consumer subscribe options

| Option (Conn.JetStream) | Desc | Interest scope | # Per-msg delivery pattern | 
| --- | --- | --- | --- |
| Subscribe | Callback handler function for each message received | consumer | N clients |
| SubscribeSync | Can check .Pending and use .NextMsg | consumer | N clients |
| QueueSubscribe | Same as Subscribe, but a "queue" group | consumer + group | 1 of N clients per group|
| QueueSubscribeSync | Same as SubscribeSync, but a "queue" group | consumer + group | 1 of N clients per group|
| ChanSubscribe | Same as Subscribe but a Channel for messages instead of handler function | consumer | N clients |
| PullSubscribe | Similar to SubscribeSync except req/rply pattern with Consumer endpoint. Use .Fetch | consumer | 1 of N clients (per Ack'd message) |