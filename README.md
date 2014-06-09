clean-code
==========
Clean code by Robert C. Martin

# Ch. 1 #

- What a bad code can make ?
* In late 80s, a company made a "killer" app. It was popular and alot of professionals bought it and used it. They had rushed the product to market and had made a huge mess in the code. As they added more and more features, the code got worse and worse until they simply could not manage it any longer. It was the bad code that brought the company down.

Having a team that are writing messy code, will decrease the productivy of the whole project. As other developers will be trying to read and understand what that code does.
```
|.
| .
|  .                      
|    .                    
|      .                  
|        .                
|           . ............ 
|_______________________________________ time
Productivity vs. time
```


- What is a clean code ?
* "I like my code to be elegant, efficient, and hard for bugs to hide. Clean code does one thing well", Bjarne Stroustrup, inventor of C++ and author of The C++ Programming Language

* "Clean code is simple and direct. Clean code reads like well-written prose." Grady Booch, author of Object Oriented Analysis and Design with Applications

* "Clean code can be read. Clean code should be literate", Dave Thomas godfather of the Eclipse strategy.

* "Clean code always looks like it was written by someone who cares" Michael Feathers, author of Working Effectively with Legacy Code

* "Reduced duplication, high expressiveness, and early building of, simple abstractions" Ron Jeffries, author of Extreme Programming

* "You know you are working on clean code when each routine you reads turns out to be pretty much what you You know you are expected" Ward Cunningham inventor of Wiki


- How to measure a goode code ? WTFs/minute, the more WTFs, the worse that code.


=======================================================

# Ch. 2 : Meaningful Names #

- Use Intention-Revealing Names:
The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.
* Bad code
`int d; // elapsed time in days`
* Good code
```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```
* Bad code
```java
public List<int[]> getThem() {
	List<int[]> list1 = new ArrayList<int[]>();
	for (int[] x : theList)
		if (x[0] == 4) 
			list1.add(x);
	return list1;
}
```
* Good code
```java
public List<int[]> getFlaggedCells() {
	List<int[]> flaggedCells = new ArrayList<int[]>();
	for (int[] cell : gameBoard)
		if (cell[STATUS_VALUE] == FLAGGED)
			flaggedCells.add(cell);
	return flaggedCells;
}
```
In that last code fragment the naming of the code is better that one before it. But the code is still hard to understand. So we can write it in another format.
```java
public List<Cell> getFlaggedCells() {
	List<Cell> flaggedCells = new ArrayList<Cell>();
	for (Cell cell : this.gameBoard)
	if (cell.isFlagged())
		flaggedCells.add(cell);
	return flaggedCells;
}
```

- Use Pronounceable Names
	
	* Bad code

		```class DtaRcrd102 {
		  private Date genymdhms;
		  private Date modymdhms;
		  private final String pszqint = "102";
		 /* ... */
		};```

	* Good code

		```class Customer {
			private Date generationTimestamp;
			private Date modificationTimestamp;;
			private final String recordId = "102";
			/* ... */
		};```

- Use Searchable Names
	
	* Bad code
	```for (int j = 0; j < 34; j++) {
		s += (t[j] * 4) / 5;
	}```

	* Good code 
	```int realDaysPerIdealDay = 4;
	const int WORK_DAYS_PER_WEEK = 5;
	int sum = 0;
	for (int j = 0; j < NUMBER_OF_TASKS; j++) {
	  int realTaskDays = taskEstimate[j] *  realDaysPerIdealDay;
	  int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
	  sum += realTaskWeeks;
	}```


- Pick One Word per Concept
	`fetch, retrieve, get // as equivalent methods`
	`controller, manager, driver // confusing`

- Use Solution Domain Names
	`AccountVisitor, JobQueue // people who read your code will be programmers`



