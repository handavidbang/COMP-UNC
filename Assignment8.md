# COMP 401, Spring 2017
## Assignment 8
# Due: Sunday, April 16th, 11:59:59 PM
This assignment will require you to use the Decorator pattern. Be sure review the notes and examples from lecture 15.

Setup

Create a new project

Start with a solution to A7 Adept that properly reports PlateEvent objects to registered Observer objects. If you were unable to get A7 Adept working, continue working on A7 Adept until you do have it working first and then attempt this assignment.

Copy your a7adept package to your new project as a8adept. You will also have to copy over the comp401.sushi package to your new project.

Adept

Modify Belt to keep track of how "old" each plate on the belt is by using the Decorator pattern to add additional state/methods to the Plate objects placed on the belt. A plate's age on the belt is the number of rotations that have occurred since the plate was placed.

Think about how you might keep track of how many rotations have occurred since a particular plate has been placed. One way to do this is to associate a counter with each plate that keeps track of the number of rotations. You might imagine a "decorated" plate that provides a way of incrementing and retrieving the count. The belt would increase the counter for each plate as part of executing a rotation.

Another possibility is to have the belt maintain a single counter of rotations as part of the belt's state. The "decorated" plate is then associated with the value of the counter when the plate was placed on the belt. Now the age of a plate can be calculated as the difference between the current value of the belt rotation counter and the value of the counter at the time the plate was placed.

Your solution should be sure to "wrap" the Plate objects placed on the belt into a decorated object when they are placed on the belt, and to "unwrap" them when they are returned as results from Belt methods such as getPlateAtPosition and removePlateAtPosition.

Modify Belt to provide the following method:

public int getAgeOfPlateAtPosition(int position)
This method should return the age of the plate at this position. If there is no plate at that position, this method should return -1.

Jedi

Make a copy of your a8adept package and rename it a8jedi

Modify PlateEvent so that it supports a new event type identified by the enumeration symbol: PLATE_SPOILED

Modify Belt to inform any registered Observer objects about plates that are spoiled using the new PlateEvent type. The rules for spoiling are as follows:

A plate with sushi that contains shellfish spoils after it has gone around the belt at least 1 time (i.e., if the age of the plate is equal to or greater than the size of the belt).
A plate with sushi that does not contain shellfish but is not vegetarian spoils after it has gone around the belt at least 2 times.
A plate with vegetarian sushi on it spoils after it has gone around the belt at least 3 times.
After each rotation, the Belt object should check each plate for being spoiled and to notify any observers with an appropriate PlateEvent object if spoiled. Once a plate is spoiled, it remains spoiled, so as long as it continues to be on the belt, it will generated a PLATE_SPOILED type PlateEvent after each rotation.

Note: the Plate object returned by the getPlate() method of a PlateEvent object should be the original Plate object placed on the belt (i.e., not your decorated object). In other words, be sure to unwrap your decorated Plate when creating the PlateEvent object sent to observers.

Create a new implementation of java.util.Observer called SpoilageCollector with the following constructor:

public SpoilageCollector(Belt b)
A SpoilageCollector object registers as an observer of the Belt object passed to its constructor and should do the following whenever a PLATE_SPOILED type PlateEvent is observed:

Remove the spoiled plate from the belt.
Keep track of the total cost of all of the spoiled sushi that it has observed.
Keep track of the total amount of spoiled shellfish (i.e., crab and shrimp) that it has observed.
Keep track of the total amount of spoiled seafood (i.e., crab, shrimp, salmon, tuna, and eel) that it has observed.
Keep track of the total amount of spoiled food (i.e., all ingredients) that it has observed.
SpoilageCollector should provide the following methods as getters for these amounts:

public double getTotalSpoiledCost()
public double getTotalSpoiledShellfish()
public double getTotalSpoiledSeafood()
public double getTotalSpoiledFood()
Turning In The Assignment

Upload to Sakai a single JAR file containing your a8adept and a8jedi packages.

The assignment is worth a total of 20 points as follows:

Adept: 10 points
Jedi: 10 points
