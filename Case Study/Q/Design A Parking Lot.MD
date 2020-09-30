Designing a parking lot (asked at Google, Facebook):

https://www.youtube.com/watch?v=DSGsa0pu8-k&t=4s&frags=pl,wn

### 1)	Handling ambiguity (recognize breadth of the question)
Ask questions, what is the interviewing looking for?

### 2)	Systematic approach
Take a step back, ask questions/clarity assumptions (details were left out purposefully)
-	How is the parking lot designed?
-	How many spots does the parking lot have?
-	Multiple levels (need to get car from one level to another)
-	Price strategy for different parking lots? Premiums? Parking for people with disability

### 3)	4 sizes parking spots
Small, medium, large, x-large
Assume you can put a small car into a big spot

### 4) Coding part
Abstract vehicle (class)
-	String license plate
-	Enum color 
Four different types of cars: car, motor cyle, bus, truck (different class objects)

```Python
class ParkingLot(zipCode: int):
    # search time O(1)  
    - Spot: placeVehicle(vehicle:vehicle) # put in a hashmap 
    
    # search time O(1)
    - Spot: removeVehicle(vehicle:vehicle) # also lookup hashmap

class Spot(id:Long, size:enum):

```

Coding part: partial solution to the whole problem, possibly using algorithm

- Backend: database to store information about vehicles, spots

- retrieve the first available spot the vehicle fits
- use four stacks to maintain the four types of available spots

- fast lookup ---> hashmap, O(1) time complexity

dive a bit deeper:
- concurrency (multiple entries)
- multiple cars trying to reserve the same spot
- pricing strategy

Ask questions and take hints (to clarify what the interviewer is looking for?)
### 5) Test 
