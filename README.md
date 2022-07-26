# Lift Algorithm

[![the-problem](https://img.shields.io/badge/-The%20Problem-58D68D)](#The-Problem) | 
[![extensions](https://img.shields.io/badge/-Extensions-ABEBC6)](#Extensions) | 
[![key-information-(medicine-chest)](https://img.shields.io/badge/-Key%20Information%20(Medicine%20Chest)-3498DB)](#Key-Information-(Medicine-Chest)) | 
[![outline-plan-(medicine-chest)](https://img.shields.io/badge/-Outline%20Plan%20(Medicine%20Chest)-8E44AD)](#Outline-Plan-(Medicine-Chest)) | 
[![classes](https://img.shields.io/badge/-Classes-BB8FCE)](#Classes) | 
[![usage-examples](https://img.shields.io/badge/-Usage%20Examples-BB8FCE)](#Usage-Examples) | 
[![notes](https://img.shields.io/badge/-Notes-BB8FCE)](#Notes) | 
[![application](https://img.shields.io/badge/-Application-E74C3C)](#Application) | 
[![extension-(ritz)](https://img.shields.io/badge/-Extension%20(Ritz)-E67E22)](#Extension-(Ritz)) | 
[![key-information-and-responses](https://img.shields.io/badge/-Key%20Information%20and%20Responses-F0B27A)](#Key-Information-and-Responses) | 

## The Problem

You are the chief engineer of AVAMAE Lift Solutions Ltd, a company that has recently won the contract for building a new lift for an office block called the MedicineChest. They would like one lift installed, and have given you the following helpful bits of information:

* There are ten floors in the block.
* The people working in the offices are distributed evenly across all floors.
* People start arriving for work at around 8 AM, and everyone has gone home by 6 PM.
* The lift needs only one external call button on each floor, instead of the usual up/down buttons. Inside the lift should be a set of buttons to send the lift to a desired floor, as with any other lift.
* The lift should have a maximum capacity of eight people at any given time.

The MedicineChest have high expectations of their new lift: given the above information, can you design an efficient algorithm for the lifts operation? What factors do you need to consider? Think about the lifts behaviour when empty, when it’s called from a floor, and when people send it to a floor. General thoughts and pseudocode are all that are necessary - as chief engineer, you have juniors to do the actual coding!

### Extensions

Off the back of your successful design at the MedicineChest, the Ritz Hotel would you like you to design their new lift system. They are a much bigger client, and some new problems need addressing:

* The hotel has 30 floors, and people are no longer distributed evenly across all of them. The ground and 15th floors are much more popular, for example.
* The hotel would like four lifts installed instead of one.

How would your original algorithm work in this new situation? Would anything have to be changed?
AVAMAE Lift Solutions Ltd want to start selling lifts with two external buttons: one for going up and one for going down, to indicate which direction you want to travel. Would this change your lift algorithm?

---

## Key Information (Medicine Chest)

Plus initial Thoughts/Considerations:

* One lift
* Ten floors
* Even distribution of people per floor:
  * Implies that lift does not need to reset to a specific, heavily used level when it has no instruction.
  * Efficiency could be gained by making the lift to wait on the fifth floor when it has no instructions. This would reduce the maximum waiting time for the lift from time taken to travel 10 floors to 5.
* Operational hours are from 8am to 6pm:
  * Lift could wait at ground floor outside of these hours in expectance of users in the morning (who I am assuming will enter at ground floor).
* Single external call button:
  * The lift will not know what direction the user will want to travel in until they are in the lift.
  * This means the lift will need to stop at each floor that it is called to and then add a new floor to its journey based on the input given by the user.
* Maximum capacity of eight people:
  * Not sure how this will be regulated - could be sensing weight or facial recognition etc - assume method is unimportant for this exercise.
  * If rule is strictly in place for this and there is a method for counting passengers, then the lift shouldn't accept any new journeys until the number of passengers is less than 8.

Functionality thoughts
* Lifts have two sets of door: one belonging to the lift and one belonging to the level.
---
## Outline Plan (Medicine Chest)

### Classes

These are the classes as I've originally envisaged them. They may develop throughout the exercise.

**Lift**

|Functions      |Variables          |
|---------------|-------------------|
|openDoor()     |direction (Bool)   |
|closeDoor()    |doorOpen  (Bool)   |
|setTargetLevel()  |journey   (Array)  |
|addLevel()     |level     (int)    |
|removeLevel()  |                   |
|resetLevel()   |                   |


**PassengerCounter**
|Functions      |Variables             |
|---------------|----------------------|
|count()        |passengerNumber (int) |


**Level**
|Functions      |Variables           |
|---------------|--------------------|
|openDoor()     |levelNumber (int)   |
|closeDoor()    |doorStatus   (bool) |
|doorOpen?      |                    |

**LiftDoor**
|Functions      |Variables           |
|---------------|--------------------|
|               |open? (bool)        |

### Usage examples

These examples are were written to help plan out how the lift should work. 

**Scenario #1:** Lift is empty, lift at level 5, button pushed at level 0.
* *Create new instance of Level with levelNumber = 0*
  * level0 = new Level(0)
* *Add "level0" to lift's "journey" array*
  * lift.addLevel(level0)
* *Lift travels to the first level in the "journey" array*
  * lift.setTargetLevel()
* *Lift removes "level0" from "journey" array once the floor level == first level in journey array*
  * lift.removeLevel()
* *Doors open and set timeout for doors to close*
  * lift.openDoor()
  * lift.setTimeout(5000, closeDoor())

**Scenario #2:** Lift is empty, lift at level 5, button pushed at level 5, passanger wants to go to level 0
* *Create new instance of Level with levelNumber = 5*
  * level5 = new Level(5)
* *Add "level5" to lift's "journey" array*
  * lift.addLevel(level5)
* *Lift level == level of first item in "journey" array so item is removed from array and doors open*
  * lift.removeLevel()
  * lift.openDoor()
  * lift.setTimeout(5000, closeDoor())
* *User presses level 0 button*
  * Follow same steps as scenario 1

### Notes

* The lift interaction and movements are abstracted out into the main (App) class
* The lift follows a journey defined by the 'journey' array, which is an ordered list of floors
* The order of the journey is constantly evaluated as each button press event occurs
* The direction of travel determines where the lift adds levels to the journey
* The Lift class is responsible for determining the lift journey
* The LiftDoor, Level and PassengerCounter classes are injected into the lift class

## Application

```
class App {
  counter = new PassangerCounter
  liftDoor = new LiftDoor

  // create new lift instance with counter and liftDoor injected on initialisation:
    lift = new Lift(counter, liftDoor)

  // Create an event halndler for when a button is pushed to call the lift,
  // or when a button is pushed inside the lift.
  // Button should know the level it's assigned to to create a new instance of Level class:
  onButtonPush{
    lift.addLevel(buttonLevel)
  }

  // Event handler for journey:
  case
    when lift.journey != []
      lift.setTargetLevel()
      lift.move()
    when lift.journey == [] && lift.liftLevel != lift.resetLevel
      lift.addLevel(resetLevel)
  end

  // Event handler for when lift reaches target level:
  when lift.liftLevel == lift.tagetLevel {
    lift.openDoor(lift.journey[0])
    setTimeOut(5000, lift.closeDoor(lift.journey[0]))
    .then(lift.removeLevel())
  }
}
```

```
class Lift {
  initialize(counter, liftDoor) {
    // instance variables to be used throughout class:
    liftCounter = counter
    liftDoor = liftDoor
    journey = []
    liftLevel
    targetLevel = nil
    direction = nil
  }

  openDoor(levelDoor) {
    liftDoor.open()
    levelDoor.open()
  }

  closeDoor(levelDoor) {
    liftDoor.close()
    levelDoor.close()
  }

  addLevel(level) {
    if level is not in array
      case
        when direction == nil
          journey.add(level)
          checkDirection()
        when direction == 'up'
          journey.add(level)
          upJourney = mapUpJourney(journey)
          downJourney = mapdownJourney(journey)
          journey = upJourney + downJourney
        when direction == 'down'
          journey.add(level)
          upJourney = mapUpJourney(journey)
          downJourney = mapdownJourney(journey)
          journey = downJourney + upJourney
        when level.levelNumber == liftLevel
          // this is here to catch when someone enters the lift and presses the level they are on
          journey.insertAtIndex(level, 0)
      end
    end
  }

  setTargetLevel {
    checkDirection()
    if direction == nil || liftDoor.open? == true
      targetLevel = nil
    else
      // move to first level in journey array
      targetLevel = journey[0]
    end
  }

  move(targetLevel = targetLevel) {
    // function controlls lift mechanism to move to target level
    // default argument to be targetLevel instance variable

    liftCounter.exceedsMax? //return warning : //move to level
  }

  removeLevel {
    journey.deleteItemAtIndex[0]
  }

  resetLevel {
    8.00 < currentTime < 18.00 ? return new Level(5) : return new Level(0)
  }

  private

  mapUpJourney(journey) {
    // map across journey array and remove floors less than liftLevel
    // order array (smallest to largest)
  }

  mapDownJourney(journey) {
    // map across journey array and remove floors greater than liftLevel
    // reverse order array (largest to smallest)
  }

  checkDirection {
    case
      when journey == []
        direction = nil
      when journey[0] > liftLevel
        direction = "up"
      when journey[0] < liftLevel
        direction = "down"
      end
  }
}
```

```
class LiftDoor {
  initialize {
    doorStatus = "closed"
  }

  open? {
    doorStatus == "open" ? true : false
  }

  openDoor {
    // controls lift door mechanism to open
    doorStatus = "open"
  }

  closeDoor {
    // controls lift door mechanism to close
    doorStatus = "closed"
  }
}
```

```
class Level << inherit from LiftDoor {
  // inheritance gives the same variables and functions to level as LiftDoor

  initialize (levelNumber) {
    levelNumber = levelNumber
  }
}
```

```
class PassengerCounter {
  //links to device for measuring number of passengers in lift. 
  //allows max to be changed, default = 6.

  initilize(max = 6) {
    max = max
    count = nil
  }

  exceedsMax? {
    // counts number of passengers with device
    count > max ? true : false
  }
}
```

## Extension (Ritz)

Off the back of your successful design at the MedicineChest, the Ritz Hotel would you like you to design their new lift system. They are a much bigger client, and some new problems need addressing:

* The hotel has 30 floors, and people are no longer distributed evenly across all of them. The ground and 15th floors are much more popular, for example.
* The hotel would like four lifts installed instead of one.

How would your original algorithm work in this new situation? Would anything have to be changed?
AVAMAE Lift Solutions Ltd want to start selling lifts with two external buttons: one for going up and one for going down, to indicate which direction you want to travel. Would this change your lift algorithm?

### Key Information and Responses

* Increase from 10 to 30 floors.
  * The increase in the number of floors would not affect the lift algorithm.
* Level 0 and 15 are much more frequently used that other floors.
  * The lift.resetLevel() function would need to be modified to send lifts back to popular floors.
  * Half the lifts could reset to level 0 and the other half to level 15.
* Four lifts are required instead of one.
  * A function would need to be added to App class to evaluate which lifts should be sent to which call request.
  * Lifts should be sent based on a hierarchy of information:
    - The button push matches the direction of travel of the lift (unless the lift has gone past the floor)
    - The lift has the least number of stops in its journey array between its current positon and the request.
* Two external buttons to indicate direction of travel.
  * This information would be used in the point above to determine which lift is sent to the call request.
