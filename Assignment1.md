COMP 401, Spring 2017

#Assignment 1
Due: Wednesday, January 25th, 2017, 11:59:59 PM
#A Java Programming Warm-Up

This assignment is intended to warm up your Java programming skills. It does not exercise any object-oriented programming concepts. You can (and should) simply use static public class functions and local variables. It will, however, exercise basic programming constructs such as loops and conditional execution and require you to use arrays.

The assignment is in three parts that are in increasing level of difficulty called A1Novice, A1Adept, and A1Jedi. In all three parts, I will provide a class with a main method that creates an input Scanner object (the use of which will be demonstrated in class and described below). The main method will then call the method process, providing the Scanner object as a parameter.

You are responsible, in each case, for implementing the process method as described below for each part, producing output to the console using System.out (see http://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html for its documentation).

You must match the format of the example outputs provided and should not produce any additional output of any kind. In particular, you should not print any input prompts or debugging statements.

You are free to (and in fact encouraged to) create additional static class methods to use as helper functions as you see fit (although each part can be done relatively easily without having to create any additional methods).

##Scanner

This assignment requires you to make use of a Scanner object. You can read the documentation for Scanner here: http://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html

A Scanner object is associated with a source of input. In our case, this will be keyboard input from the console. By default, Scanner will parse the input as whitespace separated tokens and provides methods for parsing the next available token as a particular type. For example, the method next() will retrieve the next token as a String. Similarly, the method nextInt() interprets the next token as an integer while nextDouble() will interpret the next token as a double value. For this assignment, you shouldn't have to use any Scanner methods other than next(), nextInt(), and nextDouble().

For this assignment, you can assume that the input will always be valid and conform to the description below. In other words, you do not have to worry about validating the input or being able to deal with unexpected input.

##Set Up

Download this jar file: http://www.cs.unc.edu/~kmp/comp401sp17/assignments/a1/a1-setup.jar
Create a new project in Eclipse called "Assignment1".
Import the JAR file in the "src" directory of the project.
This should create three packages called a1novice, a1adept, and a1jedi. Each should have a single class with a corresponding name that defines the main method and contains the process method where your code should go. Do not change the main method. As previously noted, you are free to create additional static class methods as you need.

#A1 Novice

This program reads in data that represents COMP 401 grade components for a set of students and computes the letter grade that they should receive. The first line of input will be an integer greater than 0 indicating the number of students in the class. Following this will be one line of input for each student in the following form:

First_Name Last_Name Assignment_Grade Recitation_Grade Midterm_1 Midterm_2 Final_Exam

First_Name and Last_Name are single word tokens that represent the first and last name of a student. All of the other entries on the line will be double values between 0.0 and 4.0.

To calculate the letter grade of each student, you should first calculate a weighted average (WA) of the grade components as specified in our syllabus (i.e., 40% assignment, 10% recitation, 15% for each midterm, 20% final), and then you should map that value to a letter grade as follows:

Weighted Average	Letter Grade
WA >= 3.85	A
3.5 <= WA < 3.85	A-
3.15 <= WA < 3.5	B+
2.85 <= WA < 3.15	B
2.5 <= WA < 2.85	B-
2.15 <= WA < 2.5	C+
1.85 <= WA < 2.15	C
1.5 <= WA < 1.85	C-
1.15 <= WA < 1.5	D+
0.85 <= WA < 1.15	D
WA < 0.85	F
For each student, you should print one line of output in the following form:

F. Last_Name Grade

Where F. represents the first letter initial of the first name for the student. For example, if the input to your program was this:

3
Carrie Brownstein 3.5 4.0 3.2 3.7 3.5
Corin Tucker 3.8 3.8 3.1 2.5 3.6
Janet Weiss 2.1 2.2 1.5 1.8 2.0

Your program should produce the following output:

C. Brownstein A-
C. Tucker B+
J. Weiss C

#A1 Adept

For this program, the input again represents a COMP 401 gradebook and you will calculate grades, but the format of the input is a bit different and you will have to calculate the homework and recitation components from raw scores.

The first line of input will be an integer greater than 0 that indicates the total number of assignments in the course. The next line of input will have a list of integers that indicate the total number of points each assignment is worth. Following this, will be a line with an integer greater than 0 indicating the number of students in the class followed by a line for each student in the following form:

First_Name Last_Name #_of_Recitations A1_Points ... AN_Points Midterm_1 Midterm_2 Final_Exam
The #_of_Recitations entry will be an integer between 0 and 15 indicating the number of recitations that the student attended. The entries A1_Points ... AN_Points will be double values that indicate the number of points the student received for each assignment. Remember that you do not need to validate these values and can assume that the value given will be in the range from 0 to the maximum number of points for the assignment as indicated at the beginning of the input. The Midterm_1, Midterm_2, and Final_Exam entries will be between 0.0 and 4.0 as before.

To calculate a student's grade, you will need to first calculate the recitation and assignment components on a 4.0 scale and then calculate the weighted average of the recitation, assignment, midterm, final components as you did in A1 Novice.

Recitation and assignment component grades should be calculated as a percentage and then mapped to a 4.0 scale. The recitation percentage is simply the number of recitations attended divided by 15. The assignment percentage is simply the sum of the points earned across all assignments divided by the total number of assignment points possible. To convert these percentages into a 4.0 scale, use the following mapping (the same as I showed on the first day of class):

Percentage	4.0 Scale
>= 95%	4.0
90%	3.5
80%	2.5
70%	1.5
<= 40%	0.0
A percentage that is in between these points should be interpolated linearly. For example, 85% should be mapped to 3.0 and similarly 55% would be mapped to 0.75.

Your program should produce output in the same form as before in A1 Novice.

Example input:

5
5 10 10 10 25
3
Carrie Brownstein 10 5 7 8.5 10 21 3.2 2.7 3.4
Corin Tucker 15 0 10 10 8 20 2.2 2.8 3.5
Janet Weiss 12 4 9 9.5 7.5 22.5 3.8 3.5 3.6

Corresponding output:

C. Brownstein B
C. Tucker B
J. Weiss B+

#A1 Jedi

For this final program, the input will be in the same format as A1 Adept except that the midterm and final exam scores will be given as raw integer scores. This means you will have to convert those raw scores into a 4.0 scale grade by calculating the appropriate exam curve.

To calculate the curve for an exam, first calculate the average and standard deviation for that exam. Here is a web page the explains how to calculate a standard deviation:

https://www.mathsisfun.com/data/standard-deviation-formulas.html

To calculate a square root, use the Math.sqrt() method.

Once you have the average and standard deviation, you can calculate a "normalized" score for each student using the following formula:

Normalized = (Raw - Average) / StdDev
Now use the following mapping to convert the normalized score to a curved 4.0 scale:

Normalized	Curved 4.0
>= 1.0	4.0
0.0	3.0
-1.0	2.0
-1.5	1.0
<= -2.0	0.0
Again, in between values should be interpolated linearly.

For output, instead of printing the name and grade of each student, print out the number of students that received each letter grade as per the example below.

Example input:

5
5 10 10 10 25
5
Carrie Brownstein 10 5 7 8.5 10 21 68 75 94
Corin Tucker 15 0 10 10 8 20 80 77 90
Janet Weiss 12 4 9 9.5 7.5 22.5 64 76 50
Polly Perfect 15 5 10 10 10 25 90 100 100
Frank Failure 5 5 5 5 5 5 40 50 50

Example output:

A : 1
A-: 0
B+: 0
B : 2
B-: 1
C+: 0
C : 0
C-: 0
D+: 0
D : 0
F : 1
##Hints

Novice and Adept can be done without using arrays and by simply producing the output for each student as your read in the data for each student within a for loop.

Jedi is easiest to do by creating an array for each of the different grade components (recitation, assignment, raw midterm 1, raw midterm 2, and raw final exam) with each student represented by a different index (i.e., 0 is the first student, 1 is the second student, etc.)

As you read in the input, calculate the recitation and assignment components as you did in Adept and store them in the corresponding array while storing the raw midterm and final scores in their corresponding arrays.

Once you have processed all of the input, go back and calculate the average and standard deviation for the midterms and final from the values in those arrays. Now you can go back through one more time and for each student calculate the curved exam scores using the average and std. deviation and combine that with the assignment and recitation components in order to arrive at a letter grade for each student. As you do so, keep track of the total number of each letter grade so that you can produce your output at the end.

##Grading

The assignment is worth 10 points as follows:

4 points for A1Novice
4 points for A1Adept
2 points for A1Jedi
Submitting Your Code

Instructions for submitting your code will be forthcoming.
