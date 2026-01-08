# The "Zombie" Consumer (Consumer Lag)

## The Situation
A consumer in a group becomes very slow or "hangs" due to a code bug or heavy processing, but it doesn't technically "crash."

## Effect
The Consumer Group Coordinator notices the consumer isn't sending "heartbeats." Eventually, it marks the consumer as dead.

## Impact
A Rebalance is triggered. The partitions assigned to the "Zombie" are taken away and given to healthy consumers. This causes a temporary pause in processing for the entire group while everyone gets their new assignments.