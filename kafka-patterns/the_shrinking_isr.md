# The "Shrinking" ISR (In-Sync Replicas)

## The Situation
You have 3 replicas, but 2 brokers start experiencing high network latency.

## Effect
The Leader detects that the 2 followers are no longer "caught up." It kicks them out of the ISR list.

## Impact
If your producer is configured with acks=all, it will still work as long as the Leader is alive. However, your durability is now at risk. If that one remaining Leader broker fails before the others catch up, you have a "clean leader election" failure, and the partition goes offline to prevent data loss.