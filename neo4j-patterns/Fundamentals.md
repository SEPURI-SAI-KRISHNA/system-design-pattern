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
#### NOTE: Here since Relation FRIEND is same from x->y and y->x, which makes this symmetric.



## Who knows Whom

```
MATCH (p:Person {name: "Rahul"})-[r]->(other)
```
#### HERE we are trying to find any node with any relation to node that has property name Rahul.

```
MATCH (p:Person {name: "Rahul"})-[r:FRIEND]->(other)
```
#### HERE we are trying to find any and all nodes with relation FRIEND to node that has property name Rahul.


## UPDATE

#### Lets say we have a node with a certain property and we want to change it

```
MATCH (p:Person {name: "Rahul"}) SET p.job="Senior Data Engineer" RETURN p
```


## Multi-Label

#### Lets say we have a node that can have multiple labels

```
MATCH (p:Person {name: "Rahul"}) SET p:Developer RETURN p
```

#### Now we can search Rahul by either Person / Developer label.


## WHERE

```
MATCH (p:Person) WHERE p.age > 28 RETURN p.name, p.age
```

#### If a node doesn't have an age property, the query simply ignores it (it treats null as not matched)



## Weighted Edge

```
MATCH (p:Person {name: "Rahul"})-[r:FRIEND]-(q:Person {name: "Rohan"})
SET r.since = 2020
RETURN p, r, q 
```

#### Here we are taking a particular node-to-node relation and adding properties.
#### There is no arrow > or < while defining relation. This means "Look for the relationship regardless of direction."



## Detach DELETE

```
MATCH (p:Person {name: "Rohan"}) DETACH DELETE p
```

#### This is the most common error for beginners. In a graph, you cannot delete a node if it still has relationships attached to it. You must cut the wires (relationships) before you remove the node. Neo4j provides DETACH DELETE to do both in one shot
