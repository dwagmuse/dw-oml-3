# Interface and Connection Manager Demo

This is intended to demonstrate an interface manaager application like CAESAR.  We borrow some basic vocabulary from openCAESAR (modified a bit -- see below)

## Purpose and Function

Managing the interconnections in a system can be challenging when there are many individual connections required and many types of connections (e.g., power, data, radio...) and all of these connections cross subsystem boundaries (specifically, domains of implementing organization authority). The fact that connections cross authority boundaries raises responsibility to the system (parent work package) to coordinate both to ensure completeness and consistency of specification and because the system is going to have to eventually integrate and test those connections.

Here we focus on modeling *functional* interfaces and connections (that is, signal flows related to system function or purpose rather than their implementation in physical wires, connectors, and pins) in order to specify function and purpose before considering the implementation details. System engineers may also care about the physical realization details of each functional connection but in many cases the work of assigning functional signals to physical connectors becomes a simpler job once the functional signals are clearly specified.

The key vocabulary used here is as follows:
* A Component is a deliverable element that performs some specified Function
* Components can present Interfaces
* Junctions specify the need to deliver signals between Interfaces
* Interfaces and Junctions can be typed by different signal types and rules established constraining type consistency between joined Interfaces and Junctions

## Interface Specification Process

During the architecting process a project will identify key functions the system needs to perform, decompose those into functions that can be implemented, and organize the functions into cohesive groups that can be allocated to different implementing organizations so that the work of implementation can proceed concurrently on each such subsystem. The system still depends on being able to functionally integrate the separate subsystems so that they can communicate signals between them to coordinate system functionality.

Most connection needs are elaborated collaboratively and iteratively between system and subsystem. For example, the system specifies the need for a radio to communicate externally. That implies the need for some kind of radio external connection. The system and subsystem will then elaborate the need for a way to internally recover the information demodulated from the radio signal as data, and supply the radio with power. Each of those interfaces on the radio then become demands that have to be satisfied by the system to connect those signals somewhere: data to where it will be interpreted, and power from a power source.

The process of elaborating and satisfying such connection demands views each unconnected interface as a need to be satisfied by a completed junction. What "completed" means can vary by interface type.

System engineers typically manage a burndown list of interfaces that have to be specified, designed, implemented, integrated, and finally tested. This application aims to help develop and manage that list in the form of a model.

The model needs to support an incremental specification process where a Junction can first be identified as joining subsystem to subsystem (e.g., we assume that a subsystem will need power and so allocate a junction from the power subsystem to the instrument subsystem to identify the need to elaborate details). As each connected subsystem design matures they can allocate the junction to specific Components and Interfaces within the subsystem. The Junction is not considered to be fully specified until it joins a minimum number of Interfaces.

## Model Functionality

A key observation from CAESAR was that the work can be facilitated through the use of common interface and component type libraries and predefined connectivity rules so that system engineers don't have to spend their time encoding all the rules and can instead spend their time specifying interfaces. Standardizing such a library is tricky because different organizations are likely to have established conventions that may vary in key details. Thus, we want to avoid hard-coding such standards and rules into the application core and try to keep this logic in plugin libraries that users can modify or extend.

### Component List

Since Interfaces are presented (owned) by Components, we need to first manage the Component composition of the system.  Components are allocated to Subsystems (functional compositions of Components) that can be allocated to a single supplier Work Package.

We don't really care about physical composition (containment) in this model -- only functional composition. So there is no relation for physical composition but there are both Subsystem and other functional aggregations (may include fault containment regions, or other functional groups that have shared functionality).

The UI needs to provide a component list view that can be organized by different grouping properties (subsystem, e.g.). User needs to be able to view the set of presented interface for a selected component (doesn't need to be a hierarchical table like CAESAR as long as user can drill down to view a components interfaces, and then drill into details of each).

#### Component Construction Wizard

We want to be able to construct instances of components according to library patterns: e.g., a given library "type" component needs to present some particular set of typed interfaces. A Component doesn't have to specialize a type and a type can be "applied" retroactively to an existing component (as a refactoring operation). A component at minimum must be a member of a subsystem, have a name and a unique key ID (auto-assigned by rule?)

The construction wizard should be able to both instantiate canonical library interfaces from the library pattern, add ad-hoc interfaces, or remove/edit interfaces.

Each interface may have distinct properties that also need to be editable.

Interfaces can also be grouped into Banks which can have properties that indirectly apply to all interfaces within the bank group.

### Connection (Junction) List

Another main view depicts the list of Junctions in the model. A given junction needs to join at least two interfaces, though those need not be in different subsystems (every Out must go to some In and every InOut must join to something).

The connection list should show for each junction its set of joined subsystem:component:interfaces. User should be able to hyperlink from the component or interface to the selected element and see its details.

## Analytical Reports

* Component List
* Connection List
* Connection Specification Burndown List (work to go)
* Filtered Connection List (by type, subsystem, ...)
* Model Dashboard (metrics for work to go)
* ...


## Vocabulary and Modifications from CAESAR

* renamed mission -> system to clarify that this is about the subject of requirements which is more generally the system being delivered/operated. The system may have a mission (goals) that can also be subject to requirements but not all systems have missions.

* simplified base to remove containment and aggregation as generalizations. Those were inherited from UML but are not necessary or even helpful in OML where relations can more easily be distinct. Avoiding those generalizations also avoids a lot of semantic conflict.



