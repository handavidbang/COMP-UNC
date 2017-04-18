# COMP 401, Spring 2017
## Assignment 9
# Due: Wednesday, April 26th, 11:59:59 PM
## Setup

Create a new project

Download and import this JAR file: a9-setup.jar

You should now have the following packages:

comp401.sushi
sushigame.game
sushigame.model
sushigame.view
sushigame.controller
This code contains a complete, functional sushi restaurant game. You should be able to run and play the game starting from the class sushigame.game.SushiGame. You should see an interface that includes a scoreboard, a visualization of a sushi belt, an interface for creating a few specific types of sushi on specific plate colors placed nearest to a specific position, a rotate button, and a message area where error messages are displayed. In this game, there are 20 positions on the belt, 5 customers sitting evenly spread out at different positions, and 4 opponent sushi chefs. Each chef is allowed to (but not required to) make and place one plate of sushi per rotation. Each chef starts with $100 in their account. When a plate of sushi is made a placed on the belt, the cost of the ingredients is subtracted from the chef's account balance. When the rotate button is pressed, any spoiled sushi is removed from the belt. Then each customer evaluates the plate in front of them (if any) and may or may not decide to consume that plate. If they do consume the plate, the chef who made the plate is credited with the price of the plate.

Read through the code and try to understand its structure and how the various components interact. Note that there have been some changes to the basic interfaces and classes in comp401.sushi. For example, each plate object is now associated with and can report the chef that made it. Another important change is that the Belt interface includes a registerBeltObserver method for registering objects that want to be informed about what happens on the belt. These belt observers are delivered BeltEvent objects which represent rotation, plates being placed on the belt, plates being consumed by a customer, and plates spoiling on the belt. Spoiled plates are removed from the belt automatically by the belt implementation. Take the time to read through all of the interfaces and classes carefully and see how they fit together. I am purposefully not going into detail here because being able to figure out how this code works is part of the assignment. Feel free to discuss with your fellow students.

Modify the code to add the following features. Each feature is worth a different number of points. This assignment will be graded by hand with the L/TAs playing your game to confirm that the features work and do not crash the game. Overall, there are 30 points available.

# A Better Belt View (10 points)

The class sushigame.view.BeltView provides a user interface for displaying the contents of the sushi belt. Currently it uses a simple JLabel for each belt position that displays the default toString() representation of the Plate object (if any) at that position. It also makes the background of the JLabel the same color as the plate.

Rewrite BeltView to be more sophisticated so that for each plate on the belt, the following information is conveyed in some reasonable way:

Color of the plate
Type of sushi (sashimi, nigiri, or roll)
If sashimi or nigiri, what kind.
If roll, the name of the roll, what ingredients and how much of each ingredient is included
The name of the chef who made the plate
The age of the plate
Although you can probably do this by just replacing the string used for the JLabel in the current BeltView class with a longer one that contains all of this information, doing so will only receive 8 points. For full credit, develop some sort of "plate view widget" which is able to convey this information more elegantly and modify BeltView to use your new widget instead of a JLabel.

# A Better UI For The Player (15 points)

In the current version of the game, the class PlayerChefView is a widget that provides the ability for the player to create only three specific types of Sushi on specific plate colors and placed nearest to specific places on the belt. Rewrite PlayerChefView so that it allows a player to create any kind of sushi on any color of plate and placed nearest to any spot on the belt. For gold plates, the player should be able to select a price between $5.00 and $10.00. For rolls, the player should be able to include up to (but not more than) 1.5 ounces of any ingredient type.

# A Better Scoreboard (5 points)

Currently the scoreboard displays the chefs in the order of their account balance from highest to lowest. Modify the game so that each chef also keeps track of the total amount of food consumed by customers and the total amount of food spoiled on the belt. (HINT: this will require you to change the interface Chef and implementation ChefImpl in the sushigame.model package.). Then rewrite ScoreboardWidget so that one can select between showing the chefs in balance order (highest to lowest), food sold order (highest to lowest) or food spoiled order (lowest to highest).

Export all the code from all the packages into a single JAR file and upload to the autograder.
