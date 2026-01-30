### The Factory Method Scenario: "Just Get Me a Vehicle"

#### The Goal

You simply need to move a package from A to B. You don't care about the driver, the insurance, or the maintenance. You just need the one correct vehicle.

#### The Problem

Your code has a generic plan_delivery() function. Sometimes it runs on land, sometimes on water.

#### The Solution

You define a method create_transport().

---

- If you use the RoadLogistics class, it overrides that method to give you a Truck.

- If you use the SeaLogistics class, it overrides that method to give you a Ship.

#### Key Characteristic

You get one object (The Vehicle).

```
Logistics App
   |
   +-- RoadLogistics Class  --> returns ONE thing: [Truck]
   |
   +-- SeaLogistics Class   --> returns ONE thing: [Ship]
```


### The Abstract Factory Scenario: "Build Me an Entire Fleet Branch"

#### The Goal

You are setting up a completely new logistics hub. You can't just buy a vehicle; you need the whole ecosystem that matches that vehicle.

#### The Problem

If you buy a Truck, you also need to hire a Driver with a commercial license, and you need Road Insurance. It would be a disaster if your system gave you a Ship, but hired a Truck Driver and bought Road Insurance.

#### The Solution

You create a "Factory" that bundles these families together.

---

- RoadFactory: When asked, it gives you a {Truck, TruckDriver, RoadInsurance}.

- SeaFactory: When asked, it gives you a {Ship, Sailor, MaritimeInsurance}.

#### Key Characteristic
You get a family of matching objects (Vehicle + Driver + Insurance).

```
Logistics App
   |
   +-- RoadFactory Object
   |      |-- create_vehicle() --> [Truck]
   |      |-- hire_driver()    --> [TruckDriver]  <-- Matches the Truck!
   |      |-- get_insurance()  --> [RoadPolicy]   <-- Matches the Truck!
   |
   +-- SeaFactory Object
          |-- create_vehicle() --> [Ship]
          |-- hire_driver()    --> [Sailor]       <-- Matches the Ship!
          |-- get_insurance()  --> [SeaPolicy]    <-- Matches the Ship!
```

### The "Oh!" Moment
To distinguishing them, ask yourself: "Am I creating just one thing, or a matching set of things?"

- Factory Method: "I need to deliver this box. RoadLogistics, give me your vehicle!" -> Gets a Truck.

- Abstract Factory: "I am setting up a sea transport division. SeaFactory, give me everything I need!" -> Gets a Ship, a Captain, and Boat Repair Tools.