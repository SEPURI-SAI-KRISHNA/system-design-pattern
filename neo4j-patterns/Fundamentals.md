## Confused ?

```
vertex - node / circle
edge - line / relation
```


## CREATE a node
### note you need to return something always, as always creating / retrieving is done by some temp variable.
```
CREATE (p:Person {name: "Anjali", age: 28, job: "Developer"})
RETURN p

CREATE (q:Person {name: "Rahul", age: 30, job: "Tester"})
RETURN q
```

## GET a node

```
MATCH (p:Person {job: "Developer"}) return p
```
#### This will return all nodes that have property job as Developer in any nodes

## Connecting nodes

### Uni-directional

```
MATCH (x:Person {name: "Rahul})
MATCH (y:Person {name: "Anjali"})
CREATE (x)-[:FRIEND]->(y)
RETURN x,y
```

### Bi-directional


```
CREATE (r:Person {name: "Rohan", age: 39, job: "CTO"})
RETURN r
```

```
MATCH (x:Person {name: "Rahul})
MATCH (y:Person {name: "Rohan"})
CREATE (x)-[:FRIEND]->(y), (y)-[:FRIEND]->(x)
RETURN x,y
```