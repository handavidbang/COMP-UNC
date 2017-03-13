COMP 401, Spring 2017

#Assignment 4
#Due: Sunday, February 26th, 11:59:59 PM
In this assignment, you will create a set of classes that will be the basis for several assignments going forward. The classes developed in this assignment will eventually be used to create a game in which a sushi chef tries to maximize profit by making and selling sushi to customers in a sushi boat restaurant. If you have never eaten sushi or don't know what a "sushi boat" restaurant is, you should check out Kurama Express on Columbia St. near Franklin (next to Buns) to get an idea of what one looks like. Sushi is a type of Japanese cuisine, typically made out of raw seafood, rice, seaweed, and sometimes other ingredients. A sushi boat restaurant is one in which pieces of sushi are placed on colored plates and the plates are placed on a circular conveyer belt. This is often a moat of water with little boats on it strung together acting as a conveyor belt, hence the name "sushi boat". Customers are seated around the conveyor and pick up the plates they want as they go by. Different plates are assigned different prices.

##Setup

In Eclipse, create a new project for the assignment. Create a package called a4novice.

#A4 Novice

First, create a public interface called Ingredient with the following methods:
```
String getName() 
double getCaloriesPerDollar() 
int getCaloriesPerOunce() 
double getPricePerOunce() 
boolean equals(Ingredient other) 
boolean getIsVegetarian() 
boolean getIsRice() 
boolean getIsShellfish() 
```
This is pretty much the same interface that we defined to represent an ingredient in assignment 3 with a couple of new methods. Specifically, an ingredient can report whether or not the ingredient is a form of rice or shellfish in addition to whether it is vegetarian or not. Additionally, an Ingredient object can compare itself with another Ingredient object with the equals method. This method should return true if the Ingredient object passed as a parameter has the same name, the same calories per ounce, is within $0.01 in price per ounce, and has the same characteristics.

Now create eight classes which implement the Ingredient interface called Avocado, Crab, Eel, Rice, Salmon, Seaweed, Shrimp, and Tuna. The characteristics of these classes are given in the following table:

Class Name	Name	Price/Oz.	Calories/Oz.	Vegetarian?	Rice?	Shellfish?
Avocado	"avocado"	$0.22	45	true	false	false
Crab	"crab"	$0.75	36	false	false	true
Eel	"eel"	$2.18	84	false	false	false
Rice	"rice"	$0.12	37	true	true	false
Salmon	"salmon"	$0.72	56	false	false	false
Seaweed	"seaweed"	$2.95	113	true	false	false
Shrimp	"shrimp"	$0.55	39	false	false	true
Tuna	"tuna"	$1.77	48	false	false	false
All of these classes should provide a constructor without any parameters. For example, for the Avocado class, the constructor should look like this:
```
public Avocado()
```
You should employ inheritance to implement these classes. The easiest way to do this is to create an abstract parent class that encapsulates all of the information about an ingredient explicitly as fields and implements the interface (HINT: we did this with IngredientImpl in assignment 3). The subclasses would then simply provide the constructor that uses the appropriate values from the table when calling the superclass constructor.

Second, create a public interface called IngredientPortion with the following methods:
```
	Ingredient getIngredient()
	String getName()
	double getAmount()
	double getCalories()
	double getCost()
	boolean getIsVegetarian()
	boolean getIsRice()
	boolean getIsShellfish()
	IngredientPortion combine(IngredientPortion other)
  ```
Again, this is pretty much the same interface for representing a specific amount of an ingredient that we used in assignment 3 with a few methods added. In particular, the methods that report whether the underlying ingredient is a form of rice or shellfish have been added. Also, the getCalories and getCost methods should return the number of calories and price for the specific amount of the ingredient without rounding.

The combine method should have the same functionality as in assignment 3. Namely, if the ingredient portion provided as the parameter other is null, then the result of combine should just be a reference to the original ingredient portion. If the ingredient associated with other is not the same ingredient, then throw a RuntimeException. You should use the equals method that we added to the Ingredient interface to compare Ingredient objects. If the two ingredient portions are the same type of ingredient, combine should return a new IngredientPortion object which represents their combined amount.

Now create eight classes that implement IngredientPortion called AvocadoPortion, CrabPortion, EelPortion, RicePortion, SalmonPortion, SeaweedPortion, ShrimpPortion, and TunaPortion. All of these classes should provide a constructor that accepts a single parameter that indicates the amount of the portion. For example, for the AvocadoPortion class, the constructor should look like this:

```
public AvocadoPortion (double amount)
```
The constructor should throw a RuntimeException if the amount specified is not greater than 0. The Ingredient associated with each of these classes should be an instance of the corresponding ingredient class (i.e., AvocadoPortion should use an instance of Avocado as its ingredient). These classes should implement the combine method so that the appropriate subclass is used. For example, combining an instance of AvocadoPortion with another instance of AvocadoPortion should return a new instance of AvocadoPortion.

Again the easiest way to do this is to use an abstract parent class that implements an ingredient portion generally (HINT: think of IngredientPortionImpl from assignment 3) with the subclasses simply providing the appropriate constructor and any subclass specific method implementations that can not be provided generally (HINT: you'll need a subclass specific version of combine).

As you implement these classes, think about whether or not you can do so in a way that reuses the same ingredient instance for every ingredient portion of the same type. This is not necessary for full points but just a challenge.

#A4 Adept

Copy your a4novice package and rename it a4adept.

In a4adept, add a public interface called Sushi with the following methods:
```
 String getName()
 IngredientPortion[] getIngredients()
 int getCalories()
 double getCost()
 boolean getHasRice()
 boolean getHasShellfish()
 boolean getIsVegetarian()
 ```
A class that implements the Sushi interface encapsulates a set of ingredient portions that make up that piece of sushi. This interface is pretty much the same as MenuItem in assignment 3 with the addition of methods for testing whether or not any of the ingredient portions are rice or shellfish. Note that the getCalories method should sum the caloric content of the component ingredients and round to the nearest integer value. Similarly the getCost method should sum the cost of the component ingredients and round to the nearest cent.

Create the following three classes which implement the Sushi interface.

##Sashimi
A piece of Sashimi is comprised of 0.75 ounces of some type of seafood. There are five types of sashimi: tuna, salmon, eel, crab, and shrimp. The Sashimi class should define a public enumeration called SashimiType with the following definition:
 public enum SashimiType {TUNA, SALMON, EEL, CRAB, SHRIMP};
The Sashimi class should have a constructor with the following signature:
 public Sashimi(SashimiType type) 
The value of type passed to the constructor indicates what type of sashimi is being made. The name associated with a Sashimi object should be the name of the underlying seafood ingredient followed by the word "sashimi". For example, a tuna sashmi object should be produce the value "tuna sashimi" as the result of the getName method.
##Nigiri
A piece of Nigiri is comprised of 0.75 ounces of some type of seafood and 0.5 ounces of rice. There are five types of nigiri: tuna, salmon, eel, crab, and shrimp. The Nigiri class should define a public enumeration called NigiriType with the following definition:
 public enum NigiriType {TUNA, SALMON, EEL, CRAB, SHRIMP}; 
The Nigiri class should have a constructor with the following signature:
 public Nigiri(NigiriType type) 
The value of type passed to the constructor indicates what type of nigiri is being made. The name associated with a Nigiri object should be the name of the underlying seafood ingredient followed by the word "nigiri". For example, a tuna nigiri object should be produce the value "tuna nigiri" as the result of the getName method.
##Roll
A roll is a sushi creation that may be comprised of any number of different ingredient portions. The Roll class should have a constructor with the following signature:
 public Roll(String name, IngredientPortion[] roll_ingredients) 
The name associated with a Roll object is provided as the first parameter to the constructor and the ingredient portions of the roll are provided as the second parameter. The constructor should check the validity of the ingredient portion array passed to it as well as the validity of the individual elements in the array. If the array is null or if any of the elements are null, throw a RuntimeException. You may assume that no ingredient type will be repeated more than once.
Specifically do NOT use inheritance for these classes. Implement each one as a separate class that implements the Sushi interface without using a common parent class.

As I've mentioned before, be careful about working with arrays as parameters to and from constructors and methods. in particular, your Roll constructor should not just blindly copy the array reference passed in to a private field encapsulated in your Roll class. Instead, you should use the clone method of the array to make a copy that you can store safely. Similarly, you should not simply return an encapsulated array reference in order to implement the getIngredients method. You should create a new array to return each time.

#A4 Jedi

Copy your a4adept package and rename it a4jedi.

Amend your Roll class to add the following functionality:

The constructor should detect if a particular ingredient type is repeated and combine the separate portions of a repeated ingredient type into a single portion.
The constructor should always include at least 0.1 ounces of SeaweedPortion as a component ingredient portion to represent the roll wrapper if the ingredient portion array passed to the constructor does not already include at least this much seaweed. If the ingredient portions passed to the constructor already have at least this much seaweed, you should not include any more.
#Grading

The assignment is worth 10 points as follows:

Novice: 4 points
Adept: 4 points
Jedi: 2 points
