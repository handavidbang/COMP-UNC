COMP 401, Spring 2017
# Assignment 7
## Due: Wednesday, April 5th, 11:59:59 PM
In this assignment, you will employ the Observer/Observable pattern while modifying our Belt class. This assignment only has an Adept and Jedi part (no Novice for this one). In the Adept part, you will make Belt a subclass of java.util.Observable. Belt will inform registered Observer objects whenever a plate is added or removed to the belt. You will also create observer classes that keep track of the number of plates of any given color on the belt and keep track of how much profit is associated with plates on the belt.

In the Jedi part, you will apply the pattern again, but this time in a customized manner. You will modify Belt to keep track of Customer objects that are observing plates at a particular position on the belt. Whenever a belt rotates, for each position on the belt, if a Customer object is observing that position and there is now a Plate object at that position after the rotation, the Customer object will be informed of the plate via a method specified in the Customer interface definition. Only one customer can be observing any given position and a particular customer can only observe one particular position at a time. In other words, the same Customer object can not be registered as an observer for two different positions on the belt at the same time. Further details are below.

## Setup

Create a new project.

Import the following the a7-setup.jar JAR file located here: http://www.cs.unc.edu/~kmp/comp401sp17/assignments/a7/a7-setup.jar

You should now have packages called comp401.sushi and a7adept. The comp401.sushi package is the same as in A6. In the a7adept package, you should see code for the following classes:

Belt
PlateEvent
Belt contains a starting point implementation for this assignment. The version provided here does not implement the iterator portions of A6 but does allow for circular indexing and rotation as implemented for A6. You can either begin with the version provided here or replace it with your working A6 code if you wish.

You need to modify this implementation of Belt so that it extends java.util.Observable. Whenever a plate is placed on or removed from the belt, you should notify any registered observers of the change. Do this by first calling setChanged and then calling notifyObservers. These are methods inherited from java.util.Observable. Use an instance of PlateEvent (described below) as the object that describes the change to pass to notifyObserver.

PlateEvent defines the object that will be passed as the second parameter to the update method when observers of Belt are notified that a plate has been placed or removed. Within PlateEvent is an enumeration called EventType that defines two symbols: PLATE_PLACED and PLATE_REMOVED. A PlateEvent object encapsulates one of these two values, a reference to the plate that was placed or removed, and the position where this occurs. You should read through PlateEvent.java to make sure you understand the class and how to construct new instances, but you should not have any need to modify the code there.

Now create the following two classes that implement the java.util.Observer interface that can be used to observe a Belt object:

PlateCounter
ProfitCounter
A PlateCounter object should have the following constructor:

public PlateCounter(Belt b)
The constructor should register the object as on observer of the belt specified as the parameter b provided. If b is null, throw an IllegalArgumentException.

A PlateCounter object should implement update in order to keep track of the number of plates of each color that are currently on the belt. It should also provide the following methods for retreiving that information:

public int getRedPlateCount()
public int getGreenPlateCount()
public int getBluePlateCount()
public int getGoldPlateCount()
A ProfitCounter object should have the following constructor:

public ProfitCounter(Belt b)
The constructor should register the object with the belt b as an observer. If b is null, throw an IllegalArgumentException.

A ProfitCounter object should implement update in order to keep track of the amount of profit (i.e., difference between the price of a plate and the cost of the sushi on the plate) associated with all of the sushi currently on the belt. It should also provide the following methods for retrieving that information:

public double getTotalBeltProfit()
public double getAvergaeBeltProfit()
The method getTotalBeltProfit returns the sum of all profit for all plates currently on the belt and the method getAverageBeltProfit returns the average profit per plate currently on the belt. If there are no plates on the belt, getAverageBeltProfit should return 0.0.

# Jedi

Make a copy of your a7adept package and rename it a7jedi.

Create an interface called Customer as follows:

public interface Customer {
       void observePlateOnBelt(Belt b, Plate p, int position);
}
Modify Belt so that it is able to associate a Customer with a specific position on the belt (HINT: you should add a private field encapsulated within Belt that is a belt-sized array of Customer objects). Provide the following two methods for (de)registering Customer objects at a particular position:

public void registerCustomerAtPosition(Customer c, int position)
public Customer unregisterCustomerAtPosition(int position)
The registerCustomerAtPosition should throw a runtime exception if the position is already registered with a different customer object. It should also throw a runtime exception if the Customer is already registered at a different position. Throw an IllegalArgumentException if the Customer object provided as c is null.

The unregisterCustomerAtPosition should unregister the Customer at the specified position and return a reference to that customer. This method should return null if there is no customer registered at that position.

For both of the above methods, the value of position should appropriately handle negative values and values larger than the size of the belt in the same way as when placing plates.

Modify Belt futher so that after a rotation occurs, the belt notifies all registered customers if there is now a plate at the customer's position by calling the Customer object's observePlateOnBelt method with the appropriate values as parameters. The value of position should be normalized between 0 and the size of the belt minus one.
# Grading

The assignment is worth a total of 15 points as follows:

Adept: 9 points
Jedi: 6 points
