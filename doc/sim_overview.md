## Simulation Overview

This file outlines a conceptual explanation of the model being simulated.

#### Contents

1. What is fabsim?
2. Model Assumptions
3. Mathematical Model
4. Agent Attributes & Behavior
5. Definitions of Uncommon Terminology
6. Discussion of Future Work

### What is fabsim?

fabsim is a prototype module for a simulation package meant to validate the initial assumptions of a more complex model. The overarching research goal is understand the socio-economic impact of the widespread diffusion and adoption of personal fabricators.

This prototype simulates agents acting as producers and consumers, producing and buying goods respectively. 

### Model Assumptions

In order to keep the model as simple as possible we are assuming the following:

* Consumers…
    1. Have an unlimited supply of money.
    2. Will always buy one good during their turn.
    3. Choose goods to buy "at random" (using a psuedo-random number generator).
    4. Behave in an economically rational manner. In other words when making decisions on what to buy, agents will tend towards the cheapest good. The word "tend" is used there because there are actually two possible options when choosingwhich good to buy; one involving a deterministic algorithm and one involving a roulette selection algo    rithm. The specifics of each can be found in the __doc/src_overview.md__.
    
* Goods…
    1. Are defined on a continuous spectrum using goodIDs between 0 - 1. This is used to differentiate one good from another.
    2. Are all of the same level of complexity. In other words if you were to imagine a hierarchy of goods separated by levels, where the lowest level is made up of raw resources such as silicon and aluminum used to make a more complex product such as a Macbook Pro, which would be at a much higher level, then our simulation is made up of only one level. There is no explicit differentiation between goods in terms of this type of complexity.
    
* Producers…
    1. Are either fabricators or factories. A fabriactor producer is able to hold a greater variety of goods within its inventory than a factory producer meaning when a consumer demands a good it's more likely that the fabricator producer has a good within its inventory close to what's being demanded. However, in the scenario where you have both factory and fabricator producers existing within the same set of producers for a particular simulation, fabricator producers will have a higher average good price than factory producers.
    
### Mathematical Model

The mathematical model being used to determine what consumers are going to buy is the following:

![equation](http://latex.codecogs.com/gif.latex?d_m_i%3D%281%2F%28t-m_i%29%5E2%29*%281%2Fc_m_i%29) 

Where where _t_ is the true good demanded by the consumer (a floating point value between 0 - 1), _m_ is the good closest to _t_ within a producer's inventory (also a floating point value between 0 - 1), and _c_m_ is the price of good _m_ that the producer is choosing to sell it for.

In other words, when choosing which producer to buy from, a consumer will take into account both the price of the good as well as how far the good is from what it actually demanded. The value generated by this function is represented here as _d_m_ which we call the probability density of buying good _m_ from producer _i_.

When using the deterministic buying algorithm, each consumer will choose the producer with the highest probability density value generated using this function. When using the roulette-based buying algorithm the probability density for each produer will be used as a weight, adding additional irrationality to the buying process.

### Agent Description

Within the simulation we have two different types of agents: consumers and producers. Consumers don't cuurrently have any attributes such as a unique ID or any amount of wealth but they do have the ability to decide what good to buy and from which producer to buy it from. When defining the number of consumers within your simulation, you are essentially defining how many goods will be bought by a consumer and sold by producers every time step of every simulation.

A Producer on the other hand has an inventory as well as a unique ID. A producer also has the ability to sell a good, find the average good ID and price of the goods within its inventory, and find the good closest to the good demanded within its inventory.

### Definitions of Uncommon Terminology

__Personal Fabricator__ means a producer that has the ability to produce goods analogous to something along the lines of a individual person owning a 3D Printer. By today's standards, a 3D Printer is able to manufacture goods that are more costum and thus closer to exaclty what consumers are looking for but at the same time manufacture those goods at a slower rate and a higher marginal cost.

__Factory__ means a producer that acts as a factory-style manufacturer by today's standards. A factory here has the ability to manufacture a small select number of goods - not always very close to what consumers want exactly - but they manufacture these goods at a high rate and for low marginal cost.

__Test Cases__ here a separated in three categories. In the __validate.py__ file you are able to explicitly define which test case you're looking to validate. The three test cases are the following:

1. Producers sell the same goods but for different prices
2. Producers sell different goods but for the same prices
3. Producers sell different goods for different prices

These are separated in order to define the results of the simulations are under different initial conditions.

__Scenarios__ here define additional intiail conditions that can be split into three categories just as the three test cases. Essentially, depending on the scenario the user defines, you can run a simulation where…

1. All producers are factory-style producers
2. All producers are fabricator-style producers
3. Half of the producers are factory style producers where the other half are fabricator style producers.

### Discussion of Future Work

__Dynamic Inventorties__ In order to further differentiate between a fabricator and factory-style producer each one should have a unique rate of producer. Currently inventories are static after they've been initialized but by adding a dynamic element to the inventories of producers one can simulate a more realistic manfucaturing scenario where a producer may actually not have enough goods within its inventory based on the demand placed upon it by consumers.

__Dynamic Pricing__ Based on the economic laws of supply and demand producers have the ability to update prices each timestep.

__Limited Resources__ Since economics studies the efficient utilization of finite resources, eventually the amount of goods and welath within the system should be made finite.

__Hierarchy of Goods__ Realistically there exist goods with differing levels of complexity that are more and less difficult to manufacture and vary in pricing by several magnitudes. Good complexity should eventually be introduced into the model.

__Product Network__ The production of goods follows recipes very similar to that of a catabolic biochemical network. Eventually, we would like to include a network of reactions that represents a universal knowledge of how to manufacture all goods known to man (theoretically).

