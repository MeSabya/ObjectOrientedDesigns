# How to approach Object Oriented Design Questions step by step

## Steps to approach the the problem:

### 1. Requirements [5–7 mins]

First of all identify if the interviewer is looking for a System Level Design or Object Level Design.

- talk about and write down the top requirements.
- talk about the use cases and possible other use cases on high level
- define your assumptions
- Clarify the scope that you are going to address in the interview

### 2. Use case Diagram : [3 mins]
While discussing the requirements you effectively already have identified the use cases to address. Now it’s time to trnsform it into a Use case diagram.

- identify the actors
- Identify the what the different actors can do

### 3. Activity Diagram [3 mins]
- Till now you have identified the actors and what each actors needs to do. 
- In this step you will focus on the logic how the overall flow of the system will look like.
- Writing down the flow diagram will solidify your understaing of the problem as well as it will give you a chance to correct any mistake you made.

:point_right: create the flow diagram for the main use cases

### 4. Class diagram [10 mins]
This is the main part where the interviewer is most interested in. Although if you have followed the above steps you have all the materials ready to finish this step with ease.

**a. Identify the core objects**

  Rule to identify Objects :
  
  i. Nouns in requirements are possible candidates for Objects
  
  ii. Verbs in the requirements are possible methods
  
**b. Identify the relationship between objects.**

**c. Identify whether to use abstract class or interface for abstractions**

**d. Identify if you can can use some Design Pattern**

**c. Design the class diagram**

*Possibly once you come up with the class diagram, the interviewer will ask you few questions regarding your design choices. You need to justify why you made the choices. Remember that in a design round there are no wrong answers … so defend your design. Although keep in mind the basic design principles while justifying your design.*

### 5. Code [15 mins]

Mostly the coing part is optional for an Object Oriented Design round. But Interviewer might ask you to implement some specific parts of the problem.
If you are asked to code the whole problem then prioritize :

a. Start with the Interfaces or abstract classes

b. Code for the core objects and the skeleton structures

c. Write the implementations

d. Write Junit

## References
https://github.com/donnemartin/system-design-primer#object-oriented-design-interview-questions-with-solutions
