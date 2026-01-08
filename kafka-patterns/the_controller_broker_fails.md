# The Controller Broker Fails
## The Situation
The specific broker currently acting as the Controller crashes.

## Effect
The cluster suddenly has no "brain" to manage partition leadership or administrative tasks.

## Impact
A new Controller is elected immediately (via Zookeeper or KRaft). During the few seconds this takes, you cannot create new topics or handle other broker failures. However, existing data flow (Producers/Consumers already running) usually continues uninterrupted because they already know who the leaders are.