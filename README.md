# Lift Algorithm

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

## Key Information + Initial Thoughts/Considerations

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

