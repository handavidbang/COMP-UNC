COMP 401, Spring 2017

# Assignment 6
## Due: Wednesday, March 22nd, 11:59:59 PM
## Due: Sunday, March 26th, 11:59:59 PM
In this assignment, you will create an abstraction that represents a moving belt in our sushi restaurant. A belt object is created with a fixed number of positions. Plate objects can be placed on and removed from the belt. There will also be a method to retrieve a plate object (if present) from a particular position without removing it from the belt. The belt object can be told to "rotate" which means that a plate at position p will now be located at position p+1. The belt is circular, so when the belt rotates, a plate object at position size-1 (where size is the length of the belt) can subsequently be found at position 0. This assignment will also require you to implement the Iterator design pattern in several different and interesting ways. Details are below.

## Setup

Create an Eclipse project called "Assignment6".

Download and import the following JAR file:

Link to A6 Setup JAR File
You should now have a package in your project called comp401.sushi. This package contains a model solution for A4 Adept and the A5 extra credit assignments. If you prefer, you can discard this code and use your own solutions as long as all of the interfaces and classes are appropriately defined and implemented.

# Novice

Create a package called a6novice. In this package you will create the following two classes:

Belt
BeltPlateException
Both of these classes will require the use of the Plate class from comp401.sushi so you will need to include the appropriate import statement. These classes are described more fully below.

Belt

The Belt class should provide the following constructor:

public Belt(int size)
The parameter size indicates how many positions for plates the belt provides. These positions are indexed from 0 to size-1 for the purposes of other methods described below. A negative or zero value for size should cause an IllegalArgumentException to be thrown.

The following instance methods should be supported:

public int getSize()
A getter for the size of the belt.
public Plate getPlateAtPosition(int position)
Returns the plate at the specified position on the belt or null if there is no Plate object there. If position is less than 0 or greater than or equal to the size of the belt, throw an IllegalArgumentException.
public void setPlateAtPosition(Plate plate, int position) throws BeltPlateException 
Sets a plate at the specified position on the belt. If the provided plate is null, throws an IllegalArgumentException. If position is out of range, throws an IllegalArgumentException. If there is already a plate at that position, throws a BeltPlateException (see details of that class below).
public void clearPlateAtPosition(int position) 
Sets the specified position on the belt to null. If position is out of range, throws an IllegalArgumentException.
public Plate removePlateAtPosition(int position)
Removes the plate at the specified position off the belt and returns it. If position is illegal, throws an IllegalArgumentException. If there is no plate at the specified position, throws a java.util.NoSuchElementException. You should be able to write this method using getPlateAtPosition and clearPlateAtPosition described above.
BeltPlateException

The BeltPlateException class is used by the Belt class to signal when an attempt is made to place a plate on the belt at a position that is already occupied. This class should be a checked exception. It should provide the following constructor:

 public BeltPlateException(int position, Plate plate_to_be_set, Belt belt) 
The constructor should invoke its super class constructor with an appropriate message string and encapsulate the information provided to the constructor. The class should also provide standard JavaBeans convention getters for this information with the following signatures:

public int getPosition()
public Plate getPlateToSet()
public Belt getBelt()
Adept

Make a copy of your a6novice package as a6adept and add the following functionality and classes as desribed below.

First, alter getPlateAtPostion, setPlateAtPosition, clearPlateAtPosition, and removePlateAtPosition so that any value for the position index can be considered valid. This means that these methods will no longer return an IllegalArgumentException no matter what value for position is provided.

To do this, treat the belt as if it were circular. This means that if size is the length of the belt, the position value size wraps back around to position index 0. Similarly, size+1 corresponds to position index 1, size+2 corresponds to position index 2, and so on. In the same way, the value -1 should correspond to position index size-1 (i.e., the end of the belt), the value -2 should correspond to position index size-2, etc.

The easiest way to handle this is to use the mod operator "%". For any positive value of position, the appropriate index value can be found by calculating position%size. For any negative value of position, the appropriate index value can be found by calculating position%size+size.

Second, add the following method to your Belt class:

 public int setPlateNearestToPosition(Plate plate, int position) throws BeltFullException
Sets the plate at the specified position if possible. If not possible because the position is already occupied, attempts to set the plate at the next highest position. This continues until either the plate is successfully placed on the belt or all of the positions on the belt have been found to be occupied (remember as the value of position gets higher it will eventually wrap back around to the beginning of the belt). In the case that the belt is full and the plate can not be placed, throw a BeltFullException (described below).
If successful, this method should return the position index where the plate ended up. This value should be in the range of 0 to size-1 where size is the size of the belt.
Create a BeltFullException class to support the setPlateNearestToPosition method. This class should be a checked exception. It should have the following constructor:

 public BeltFullException(Belt belt) 
A BeltFullException should encapsulate the reference to the belt object provided to the constructor and provide the following getter method:

public Belt getBelt()
Third, create a BeltIterator class that implements the Iterator design pattern for the plates on a belt as described below.

The BeltIterator class should implement the interface Iterator<Plate>. You will need to import Iterator from the java.util package. A BeltIterator will be used to iterate over all of the plates on a belt starting a particular position. When the end of the belt is reached, the iterator should go back to the beginning of the belt (remember, our belt is being treated as if it were circular). This means that as long as there is at least one plate on the belt, the iterator will never run out of plates to iterate over since it will keep going back to the beginning.

The BeltIterator should have the following constructor:

 public BeltIterator(Belt belt, int start_position) 
The parameter belt indicates which belt object is to be iterated over. The start_position parameter indicates which position index to start with. Any value should be valid for start_position as per the above description of circular indexing.

BeltIterator should implement the following methods:

public boolean hasNext()
Indicates that there is a next plate object to be iterated. This method should only return false if the belt is completely empty.
public Plate next()
Retrieve the next plate from the belt. Note that this should not remove the plate from the belt. If next is called on an empty belt, should throw a java.util.NoSuchElementException.
public void remove()
Always throws an UnsupportedOperationException.

As of Java 8, Java supports "default implementations" that can be specified in interfaces. This is different from the understanding of interfaces that I have been teaching which is that Java interfaces do not contain any implementation code. Although we will not be using default implementations in interfaces for the interfaces and classes that we design as part of our assignments, the java.util.Iterator interface provides a default implementation for the remove method which simply throws an UnsupportedOperationException. What this effectively means, is that you do not actually have to provide an implementation for remove in your BeltIterator class because the default implementation provided by the interface already does what we want. I've included the remove method here in the assignment description just to be complete in terms of the required methods for the interface because Jedi will ask you to implement this method so that it is a supported operation.
Fourth, alter your Belt class in the following ways:

Declare your Belt class as an implementation of Iterable<Plate>.

Add the following two methods to your Belt class:

public Iterator<Plate> iterator()
Returns a BeltIterator object for this belt starting at position 0.
public Iterator<Plate> iteratorFromPosition(int position) 
Returns a BeltIterator object for this belt starting at the specified position.
Finally, add the following method to the Belt class:

public void rotate()
This "rotates" the belt such that a Plate object set at position p before the rotate method, is now found at position p+1 after the rotate method. Note that while one way to do this is to actually move the plates on the belt to their new locations, there is a much more efficient solution that does not require moving the plate objects at all. Either approach is fine and if you aren't sure how you would do it without moving the plates, I would suggest first doing it that way, get it working and turned in, and then seeing if you can figure out the more sophisticated and efficient way that does not require actually moving the plates.
NOTE: the rotate functionality is not part of or related to the iterator/iterable functionality. In fact, calling rotate on a belt object could cause an existing belt iterator to skip plates.

Jedi

Make a copy of your a6adept package and rename it a6jedi and add the following features:

First, alter your BeltIterator class to support the remove operation as described in the official Java documentation for Iterator found at the link below. In this context, the remove method should remove the plate last retrieved by this iterator using next from the belt. Calling remove before next has ever been called or calling remove a second time before calling next again should result in throwing an IllegalStateException.

http://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html

Finally, create two new implementations of Iterator<Plate> called PriceThresholdBeltIterator and ColorFilteredBeltIterator that support the ability to iterate only over plates on a belt that match either a maximum price criteria or are of a particular color. The constructors for these classes should be declared as follows:

public PriceThresholdBeltIterator(Belt belt, int start_position, double max_price)
Creates a belt iterator that iterates over plates with a price less than or equal to the specified max_price starting at the position specified as start_position.
public ColorFilteredBeltIterator(Belt belt, int start_position, Plate.Color color_filter)
Creates a belt iterator that iterates over plates matching the specified color_filter value starting at the position specified as start_position.
Also overload the iterator and iteratorFromPosition methods in your Belt class so that it can be used to create these new kinds of iterators for the belt. The overloaded versions should have the following signatures:

 public Iterator<Plate> iterator(double max_price) 
 public Iterator<Plate> iterator(Plate.Color color) 
 public Iterator<Plate> iteratorFromPosition(int position, double max_price) 
 public Iterator<Plate> iteratorFromPosition(int position, Plate.Color color) 
Turning In The Assignment

Upload to the autograder as a single JAR file.
Grading

The assignment is worth a total of 15 points as follows:

Novice: 4 points Adept: 8 points Jedi: 3 points
