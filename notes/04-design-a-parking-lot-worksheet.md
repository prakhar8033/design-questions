# Design a parking lot

> A parking lot or car park is a dedicated cleared area that is intended for parking vehicles. In most countries where cars are a major mode of transportation, parking lots are a feature of every city and suburban area. Shopping malls, sports stadiums, megachurches, and similar venues often feature parking lots over large areas
[Reference](https://github.com/tssovi/grokking-the-object-oriented-design-interview/blob/master/object-oriented-design-case-studies/design-a-parking-lot.md)

![Parking Lot](
    https://github.com/tssovi/grokking-the-object-oriented-design-interview/raw/master/media-files/parking-lot.png)

> Parking lot is an open area designated for parking cars. We will design a parking lot where a certain number of cars can be parked for a certain amount of time. The parking lot can have multiple floors where each floor carries multiple slots. Each slot can have a single vehicle parked in it.
[Reference](https://medium.com/double-pointer/system-design-interview-parking-lot-system-ff2c58167651)


![Parking Lot](https://miro.medium.com/max/640/1*-6QRtfh6OrHJBb7nJvsCVA.jpeg)

## Requirements gathering

What are some questions you would ask to gather requirements?
```
1. Can a parking lot have multiple floors?
2. Can a parking lot have multiple extrances?
3. Can a parking lot have multiple exits?
4. Can a parking lot have multiple types of vehicles?
5. Can we park any type of vehicle in any slot?
6. How do we get a ticket?
7. How do we know if a slot is empty?
8. How are we allocated a slot?
9. How do we pay for parking?
10. What are the multiple ways to pay for parking?
```

## Requirements
What will be 10 requirements of the system, according to you?
Do not worry about the correctness of the requirements, just write down whatever comes to your mind.
Your job is not to generate the requirements, but get better at understanding problem statements and anticipating the functionalities your application might need.
```
1. Should have multiple floors.
2. Multiple entries and exit points.
3. A person has to collect ticket at entry and pay at exit.
4. Pay at-
    a) Exit counter (cash to the parking attenddant)
    b) Dedicated automated booth on each floor
    c) Online
5. Pay via
    a) Cash
    b) Credit
    c) UPI
6. Allow entry for a vehicle if a slot is available for it. Show on the display at entry if a slot is not available.
7. Parking slots of 3 types:
    a) Small
    b) Medium
    c) Large
8. A car can only be parked at its slot. NOt on any other (even larger).
9. A display on each floor with the status of the floor correspoing to each vehicle type.
10. Fees calculated based on per hour price. 
    a) Small
    b) Medium
    c) Large
```

## Use case diagrams

Are the requirements clear enough to define use cases?
If not, try to think of the actors and their interactions with the system.

### Actors
What would be the actors in this system?
```
Customers
Parking Attendant/ Operator
Admin
```

### Use cases

What would be the use cases i.e. the interactions between the actors and the system?

#### Actor 1

Name of the actor - `Admin `

Use cases:
```
1. Create a Parking lot
2. Create a Parking floor
3. Add new parking slots
4. Update status of a parking slot
```
#### Actor 2

Name of the actor - `Parking attendant`
Use cases:
```
1. Check empty slots
2. Issue a ticket - Allocating a slot
3. Collect payment
4. Checkout - Has the user paid?
```

#### Actor 3

Name of the actor - `Customer`
Use cases:
```
1. Pay - online/exit gate/payment booths
2. Reveive ticket
3. Park the vehicle
4. Check status 
```
Add more actors and their use cases as needed.

**Create a use case diagram for the system.**

```
@startuml
left to right direction
actor ParkingAttendant
actor Customer
actor Admin

rectangle FastAndCalm {
    Admin --> (Add a parking lot)
    Admin --> (Add a parking floor)
    Admin --> (Add a parking spot)
    Admin --> (Update status of parking spot)

    usecase "Pay" as Pay
    usecase "Pay Online" as PayOnline
    usecase "Pay Cash" as PayCash

    Customer --> (Pay)
    Customer --> (Check spot's status)

    PayOnline .> (Pay) : extends
    PayCash .> (Pay) : extends


    ParkingAttendant --> (Check empty slots)
    ParkingAttendant --> (Issue a ticket)
    ParkingAttendant --> (Collect payment)
    ParkingAttendant --> (Checkout)

    (Issue a ticket) .> (Allocate a slot) : includes
    Checkout .> (CheckPaymentStatus) : includes
}
@enduml
```

## Class diagram

What will be the major classes and their attributes?

```
    Class name
        - Attribute 1
        - Attribute 2
        ...
```

List down the cardinalities of the relationships between the classes.
```
1. Parking Lot
    a) Name
    b) Address
    c) Parking Floors
    d) Entry Gates
    e) Exit Gates
    f) Display Board
2. Parking Floor
    a) Floor Number
    b) Parking Spots
    c) Display Board
    d) Payment Counter
3. Parking Spot
    a) Spot Number
    b) Spot Type - `Large, Medium, Small`
    c) Status - Occupied, Free, Out of order
4. Parking Ticket
    a) Ticket ID
    b) Parking Spot
    c) Entry Time
    d) Vehicle
    e) Entry Gate
    f) Entry Operator
5. Receipt/Invoice
    a) Ticket
    b) Exit Time
    c) Invoice ID
    d) Amount
    e) Payment
    f) Payment Status
6. Payment
    a) Amount
    b) Ticket
    c) Type- Small, Medium, Large
    d) Status
    e) Time
7. Vehicle 
    a) License Plate
    b) Vehicle Type - Small, Medium, Large
8. Parking Attendant
    a) Name
    b) Email
```

Draw the class diagram.
```mermaid
classDiagram
    class ParkingLot {
        +String name
        +String address
        +ParkingFloor[] parkingFloors
        +ParkingGate[] entryGates
        +ParkingGate[] exitGates 
    }

    class ParkingFloor {
        +int floorNumber
        +ParkingSpot[] parkingSpots
        +PaymentCounter paymentCounter
    }

    class PaymentCounter {
        +int counterNumber
    }

    class ParkingSpot {
        +int spotNumber
        +ParkingSpotType spotType
        +ParkingSpotStatus status
    }

    class ParkingTicket {
        +String ticketId
        +ParkingSpot parkingSpot
        +Date entryTime
        +Vehicle vehicle
        +ParkingGate entryGate
        +ParkingAttendant entryOperator
    }

    class Invoice {
        +String invoiceId
        +Date exitTime
        +ParkingTicket parkingTicket
        +double amount
        +Payment payment
    }

    class Payment {
        +double amount
        +ParkingTicket ticket
        +PaymentType type
        +PaymentStatus status
        +Date time
    }

    class Vehicle {
        +String licensePlate
        +VehicleType vehicleType
    }

    class ParkingAttendant {
        +String name
        +String email
    }
    
    class ParkingGate {
        +String gateId
        +ParkingGateType gateType
        +ParkingAttendant attendant
    }

    class ParkingSpotType {
        <<enumeration>>
        Small,
        Medium,
        Large,
    }

    class ParkingSpotStatus {
        <<enumeration>>
        Occupied,
        Free,
        OutOfOrder,
    }

    class PaymentType {
        <<enumeration>>
        Cash,
        CreditCard,
        UPI,
    }

    class PaymentStatus {
        <<enumeration>>
        Done,
        Pending,
    }

    class VehicleType {
        <<enumeration>>
        Car,
        Truck,
        Bus,
        Bike,
        Scooter,
    }

    class ParkingGateType {
        <<enumeration>>
        Entry,
        Exit,
    }

    ParkingLot "1" --* "*" ParkingFloor
    ParkingLot "1" --* "*" ParkingGate : entryGates
    ParkingLot "1" --* "*" ParkingGate : exitGates

    ParkingFloor "1" --* "*" ParkingSpot
    ParkingFloor "1" -- "1" PaymentCounter

    ParkingTicket "*" --o "1" ParkingSpot : generated for 
    ParkingTicket "*" --o "1" Vehicle : generated for
    ParkingTicket "*" --o "1" ParkingGate : generated at
    ParkingTicket "*" --o "1" ParkingAttendant : generated by

    Invoice "1" --o "1" ParkingTicket : generated for
    Invoice "1" --o "1" Payment : paid by

    Payment "*" --o "1" PaymentType
    Payment "*" --o "1" PaymentStatus
    Payment "1" --o "1" ParkingTicket : paid for

    Vehicle "*" --o "1" VehicleType

    ParkingGate "*" --o "1" ParkingAttendant
    ParkingGate "*" --o "1" ParkingGateType

    ParkingSpot "*" --o "1" ParkingSpotType
    
    ParkingSpot "*" --o "1" ParkingSpotStatus

```

