# Interface Manager Demo

This is intended to demonstrate an interface manaager application like CAESAR.  We borrow some basic vocabulary from openCAESAR (modified a bit -- see below)


## Vocabulary and Modifications from CAESAR

PerformingElement is the abstract aspect for things that can have/perform functions. A component is just a physical performing element and a subsystem or functional group is abstract. So, e.g., a chassis containing cards can be modeled as a component containing other components, a subsystem is just an aggregation of performing elements and you can have other similar aggregations.

