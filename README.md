clean-code
==========
Clean code by Robert C. Martin



Ch. 1

- What a bad code can make ?
=> In late 80s, a company made a "killer" app. It was popular and alot of professionals bought it and used it. They had rushed the product to market and had made a huge mess in the code. As they added more and more features, the code got worse and worse until they simply could not manage it any longer. It was the bad code that brought the company down.

Having a team that are writing messy code, will decrease the productivy of the whole project. As other developers will be trying to read and understand what that code does.

   |.
   | .
   |  .                      
   |    .                    
   |      .                  
   |        .                
   |           . ............ 
   |_______________________________________ time
   			Productivity vs. time



-What is a clean code ?
= "I like my code to be elegant, efficient, and hard for bugs to hide. Clean code does one thing well", Bjarne Stroustrup, inventor of C++ and author of The C++ Programming Language

= "Clean code is simple and direct. Clean code reads like well-written prose." Grady Booch, author of Object Oriented Analysis and Design with Applications

= "Clean code can be read. Clean code should be literate", Dave Thomas godfather of the Eclipse strategy.

= "Clean code always looks like it was written by someone who cares" Michael Feathers, author of Working Effectively with Legacy Code

= "Reduced duplication, high expressiveness, and early building of, simple abstractions" Ron Jeffries, author of Extreme Programming

= "You know you are working on clean code when each routine you reads turns out to be pretty much what you You know you are expected" Ward Cunningham inventor of Wiki


- How to measure a goode code ?
	
	WTFs/minute, the more WTFs, the worse that code.


=======================================================

Ch. 2 : Meaningful Names

- Use Intention-Revealing Names:

	The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

	* Bad code

		`int d; // elapsed time in days`
	* Good code

		```int elapsedTimeInDays;
		 int daysSinceCreation;
		 int daysSinceModification;
		 int fileAgeInDays;``` 

	* Bad code
		```public List<int[]> getThem() {
			List<int[]> list1 = new ArrayList<int[]>();
			for (int[] x : theList)
				if (x[0] == 4)
					list1.add(x);
			return list1;
		}```

	* Good code
		```public List<int[]> getFlaggedCells() {
			List<int[]> flaggedCells = new ArrayList<int[]>();
			for (int[] cell : gameBoard)
				if (cell[STATUS_VALUE] == FLAGGED)
					flaggedCells.add(cell);
			return flaggedCells;
		}```

		In that last code fragment the naming of the code is better that one before it. But the code is still hard to understand. So we can write it in another format.

		```public List<Cell> getFlaggedCells() {
			List<Cell> flaggedCells = new ArrayList<Cell>();
			for (Cell cell : this.gameBoard)
				if (cell.isFlagged())
					flaggedCells.add(cell);
			return flaggedCells;
		}```

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



================================================================================

Ch. 3: Functions

- Small 
	```// rules of functions:
	// 1. should be small
	// 2. should be smaller than that
	// < 150 characters per line
	// < 20 lines```

- Do One Thing
	```// FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL.
// THEY SHOULD DO IT ONLY.```

- One Level of Abstraction per Function
	```// high level of abstraction
 getHtml()
// intermediate level of abstraction
 String pagePathName = PathParser.render(pagePath);
// remarkably low level
 .append("\n")```


```class Employee{
	 int payAmount() {
		 switch (getType()) {
		 	case EmployeeType.ENGINEER:
		 return _monthlySalary;
		 	case EmployeeType.SALESMAN:
		 return _monthlySalary + _commission;
		 	case EmployeeType.MANAGER:
		 return _monthlySalary + _bonus;
		 	default:
		 	throw new Exception("Incorrect Employee");
		 }
	 }
	}```
	Does more than one thing!

	```class EmployeeType...
		 abstract int payAmount(Employee emp);
	class Salesman...
 		int payAmount(Employee emp) {
 			return emp.getMonthlySalary() + emp.getCommission();
 		}
	class Manager...
 		int payAmount(Employee emp) {
 			return emp.getMonthlySalary() + emp.getBonus();
 		}```
` Each method does only one thing

- Function Arguments
	`// the ideal number of arguments for a function is zero`

- Common Monadic Forms
	```// if a function is going to transform its input argument,
	// the transformation should appear as the return value
	StringBuffer transform(StringBuffer in)
	// is better than
	void transform(StringBuffer out)
	// asking a question about that argument
	boolean fileExists(“MyFile”)
	// operating on that argument, transforming and returning it
	InputStream fileOpen(“MyFile”)
	// event, use the argument to alter the state of the system
	void passwordAttemptFailedNtimes(int attempts)```


	```// flag arguments
	void render(true)
	|--> renderForSuite()
	|->  renderForSingleTest()
	```

- Have No Side Effect
	```// do something or answer something, but not both
	public boolean set(String attribute, String value);
	setAndCheckIfExists
	if (attributeExists("username")) {
	 setAttribute("username", "unclebob");
	 ...
	}```

- Structured Programming
```// Edsger Dijkstra’s rules
// one entry
// one exit
// functions small
// occasional multiple return, break, or continue statement
// can sometimes even be more expressive Dijkstra’s rules```

===========================================================================

Ch. 4: Comments

-	Comments Do Not Make Up for Bad Code
	`// don’t comment bad code, rewrite it!`

- Explain Yourself in Code
	```// Check to see if the employee is eligible for full benefits
	if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
	if (employee.isEligibleForFullBenefits())```
`
- Clarification
	```assertTrue(a.compareTo(b) == -1); // a < b
	assertTrue(b.compareTo(a) == 1); // b > a```

======================================================================
	
	Ch. 6: Objects and Data Structures

- The Law of Demeter
	`final String outputDir = ctxt.getOptions() .getScratchDir() .getAbsolutePath();`
- Train Wrecks
	```Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
final String outputDir = ctxt.options.scratchDir.absolutePath;```

======================================================================

 Ch. 8 Classes

- Class Organization
```// public static constants
// private static variables
// private instance variables
// public functions
// private utilities called by a public function right after```
`

- Classes Should Be Small!
	```// the first rule is that they should be small
	// the second rule is that they should be smaller than that```

- The Single Responsibility Principle (SRP)
	```// a class or module should have one, and only one,
	// reason to change
	// SRP is one of the more important concept in OO design```

- Cohesion
	`// maintaining cohesion results in many small classes`
