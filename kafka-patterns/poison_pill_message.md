# "Poison Pill" Message (Offset Stuck)
## The Situation
A producer sends a message that is formatted incorrectly. The consumer reads it, tries to process it, fails, and crashes/restarts.

## Effect
Because the consumer crashed before "committing" its Offset, it restarts and tries to read the exact same "poison" message again.

## Impact
The consumer gets stuck in an infinite loop. This is a common production nightmare. The Offset never moves forward, and the Consumer Lag for that partition grows indefinitely until manual intervention (like skipping the offset or fixing the code) occurs.
