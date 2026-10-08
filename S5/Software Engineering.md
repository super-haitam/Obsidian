# Class Diagram

![[Pasted image 20261005235004.png]]

### Description
Here there are 3 classes, some attributes have a certain symbol next to them:
- `+`: Means *public*
- `-`: Means *private*
- `#`: Means *protected*

There are also links between the classes:
- **Line with arrow** means inheritance (e.g. the class Student extends Person and inherits all its attributes).
- **Multiplicity**: The * next to the class Student means each Major can be linked to many students, where as each student has exactly one Major. 

# Exercises
Draw a class diagram corresponding to the following situations.
## Exercise 1
	An organization has three categories of employee: professional staff, technical staff and support staff. The organization also has departments and divisions. Each employee belongs to either a department or a division. Assume that people will never need to change from one category to another

![[Pasted image 20261006225728.png]]

### Questions
- Why are staff class inheriting from the **Employee** class ?
	- These are hints from the description, as in: *categories of employee*, as well as, *Assume that people will never need to change from one category to another*, but this argument makes sub-classes choice as the result of hunch, that can't be right.
	- In fact, after rechecking (with Claude), the phrase *categories of employee*, is supposed to make us deduce that a category of employee is kind of employee, thus, we model it as such. The second phrase simply seems to support this design, although it seems unnecessary.
- Same for **Organizational Unit** class.
	- We have a constraint *Either Or* linking each employee to one of Division or Department. One that isn't natively taken care of in a Class diagram, and so, we try to model that way.
	- That said, we could also model it using an *association* to both *Division* and *Department* with cardinal of 0..1, however, that doesn't prevent the case where an employee could have both, or neither.
## Exercise 2
	A grocery store has some items sold by weight, and some per unit. Some items are taxable, while others are not. Some items have special prices when sold in groups (e.g. 3 for $2). Finally, some items have special prices if you have certain ‘membership cards’. There could be several different membership prices on the same item, but you can only use one membership card per purchase.

- [ ] TODO
# TODO List
- [ ] Go over: ==🟠3.2 Modeling with UML Part 2.pdf
- [ ] Exercice on page **37** 