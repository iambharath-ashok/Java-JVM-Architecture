# Java-JVM-Architecture


## JVM Memory Allocation

### How does Java allocate stack and heap memory?

-	JVM will divide the Memory into 4 Parts for Optimal utilization
	
	1.	Heap -> contains Objects (may also contain reference variables)
	2.	Stack -> contains methods, local variables and reference variables
	3.	Code -> contains  ByteCode
	4.	Static -> contains Static data/methods
	
-	Primitive variables like int, long, float, double…Etc are allocated in the stack 
-	If variables are local variables it places in stack 
-	And if variables are member variables (i.e. fields of a class) then it place in heap
-	At the time of multi-threaded application each thread will have its own stack but will share the same heap
-	So we should  take care in our code to avoid any concurrent access issues in the heap space
-	The stack is thread-safe because each thread will have its own stack with say 1MB of RAM allocated for each thread
-	But the heap is not thread-safe unless guarded with synchronization in code
- 	The stack space can be increased with the –Xss option
-	Setting the maximum Java heap size (Xmx)
-	 To limit heap size to 64 MB the option should be specified like this

		-Xmx64m
		
-	Using that memory limit setting, the complete Java command from the shell script that starts my Java program looks like this:

		java –Xmx64m -classpath “.:${THE_CLASSPATH}” ${PROGRAM_NAME}		


###	 What is Stack Memory in Java?

-	Stack Memory in Java is used for static memory allocation and the execution of a thread
-	It contains primitive values that are specific to a method and references to objects that are in a heap, referred from the method
-	Access to this memory is in Last-In-First-Out (LIFO) order
-	Whenever a new method is called, a new block on top of the stack is created which contains values specific to that method, like primitive variables and references to objects
		
		
#### Key Features of Stack Memory	

-	It grows and shrinks as new methods are called and returned respectively
-	Variables inside stack exist only as long as the method that created them is running
-	It’s automatically allocated and deallocated when method finishes execution
-	If this memory is full, Java throws java.lang.StackOverFlowError
-	Access to this memory is fast when compared to Heap memory
-	This memory is ThreadSafe as each thread operates in its own stack

-------------------------------------------

###	 What is Heap Space in Java?

-	Heap space in Java is used for dynamic memory allocation for Java objects and JRE classes at the runtime
-	New objects are always created in heap space and the references to this objects are stored in stack memory

#### This memory model is further broken into smaller parts called generations, these are:

-	Young Generation 
	
	– 	This is where all new objects are allocated and aged
	-	A minor Garbage collection occurs when this fills up
	
-	Old or Tenured Generation 
	
	– 	This is where long surviving objects are stored
	-	When objects are stored in the Young Generation, a threshold for the object’s age is set 
	-	And when that threshold is reached, the object will be moved to the old generation

-	Permanent Generation 
	
	– This consists of JVM metadata for the runtime classes and application methods

#### Key Features of Java Heap Memory
	
-	It’s accessed via complex memory management techniques that includes Young Generation, Old or Tenured Generation, and Permanent Generation
-	If heap space is full, Java throws java.lang.OutOfMemoryError
-	Access to this memory is relatively slower than stack memory
-	This memory, in contrast to stack, isn’t automatically deallocated. It needs Garbage Collector to free up unused objects so as to keep the efficiency of the memory usage
-	Unlike stack, a heap isn’t threadsafe and needs to be guarded by properly synchronizing the code	
	
-------------------------------------------

##	Where-are-static-methods-and-static-variables-stored-in-java

-	Static methods will allocated memory in Stack Area
-   Static variables will allocated memory in Method Area


-------------------------------------------

## 	What is Permanent Memory Generation/ Method Area

-	Permanent Memory Generation is also called as Method Area
-	Method Area is a place where Class Loader will load all the classes
-	Method Area is a Special part of Heap Area or may not be
-	All metadata about class like java.lang.Class representation of classes will be stored in Method Area
-	All Static Primitives and Reference will allocated memory in Method Area
-	Default size of PermGen Space of 64Mb
-	Method Area is not Thread Safe

	

-------------------------------------------
## 	How Class Level Primitive and Class Level Instance Members are Allocated

-	Class Level Primitive Variables and Primitives are stored in Heap
-	Class Level Primitive Variables and Primitives are referenced by this reference in instance method
-	In order to access in static method we need to create a new Instance and assign it to local reference variable
		
		
		class A {
			static int i=0;
			static int j;

		   static void method() {
			   // static k=0; can't use static for local variables only final is permitted
			   // static int L;
			}
		}

-	Static methods (in fact all methods) as well as static variables are stored in the PermGen section of the heap
-	Since they are part of the reflection data (class related data, not instance related)
-	Note that only the variables and their technical values (primitives or references) are stored in PermGen space
-	If static variable is a reference to an object that object itself is stored in the normal sections of the heap (young/old generation or survivor space). Those objects (unless they are interal objects like classes etc.) are not stored in PermGen space
-	Class variables(Static variables) are stored as part of the Class object associated with that class
-	This Class object can only be created by JVM and is stored in permanent generation

---------------------------------------------------------

##	How Static Primitive Variable and instance variable will be allocated

-	Static Primitive Variable will allocated memory in the Method Area or PermGen Area of Heap

---------------------------------------------------------

##	What is MetaSpace

-	From Java 8, PermGen has been replaced with MetaSpace
-	MetaSpace is dynamic will nature can grow and shrink dynamically at runtime


---------------------------------------------------------


## What does the statement “memory is managed in Java” mean?

-	Memory management will be handled by JVM automatically in Java
-	Developer don't needs to allocate and deallocate memory manually
-	The JVM and more specifically the Garbage Collector – has the duty of handling memory allocation so that the developer doesn’t have to
- 	Unless in C and C++, developer needs to allocate and deallocate memory munally


---------------------------------------------------------
## What is Garbage Collection and what are its advantages?

-	Garbage Collector is Thread in Java that looks for unused objects on the Heap Memory and removes them
-	Garbage collection 
	
	-	Is a process of looking at heap memory, identifying which objects are in use 
	-	And which are not, and deleting the unused objects

-	In-use object or Referenced object: 
	
	-	Some part of your program still maintains a pointer to that object

-	 An unused object or Unreferenced object: 

	-	Object is no longer referenced by any part of your program
	-	So the memory used by an unreferenced object can be reclaimed	
	
-	The biggest advantage of garbage collection is that:
	
	-	It removes the burden of manual memory allocation/deallocation from Developer
	-	So that Developer can focus on solving the problem at hand or functional problems

---------------------------------------------------------
##  Are there any disadvantages of Garbage Collection?

-	Yes. Whenever the garbage collector runs, it has an effect on the application’s performance
-	This is because all other threads in the application have to be stopped to allow the garbage collector thread to effectively do its work
-	This problem can be greatly reduced or even eliminated:
	
	-	Through skillful optimization and garbage collector tuning and using different GC algorithms
---------------------------------------------------------
## What is the meaning of the term “stop-the-world”?

-	When the garbage collector thread is running, other threads will be stopped, meaning the application is stopped momentarily
-	This is like cleaning house and denying access to house till cleaning gets completed
-	Depending on the needs of an application, “stop the world” garbage collection can cause an unacceptable freeze
-	This can be optimized by tuning Garbage Collector

---------------------------------------------------------
## What are stack and heap? What is stored in each of these memory structures, and how are they interrelated?

-  Stack

	-	Stack and Heap are Memory Areas that are used by JVM to run the Application
	-	Stack Area is a area in which JVM will allocate memory for Method in form of Frame
	-	Methods will be executed on the Stack Area in FIFO order
	-	All local primitive and reference variable are allocated memory on the stack
	-	Each Thread will get its own Stack Area and hence its Thread Safe

-	Heap

	-	Heap is a large bulk of memory intended by allocation of Objects 	
	-	Heap Area is Area in which all the Objects will be allocated memory
	
	
Relation b/w Stack and Heap:
	-	When object with the new keyword is created, it gets allocated on the heap
	-	However, the reference to this object lives on the stack
---------------------------------------------------------
## What happens when there is not enough heap space to accommodate storage of new objects?

-	If there is no memory space for creating a new object in Heap, Java Virtual Machine throws OutOfMemoryError 
-	More specifically java.lang.OutOfMemoryError heap space


---------------------------------------------------------
##  How do you trigger garbage collection from Java code?

-	Java Programmer, can not force garbage collection in Java
-	Java Programmer, can only request Garbage Collector to run by Calling Runtime.gc() and System.gc() nethods

	-  	But it’s not guaranteed that garbage collection will happen
	
-	Garbage Collection will only gets triggered:
	
	-	If JVM thinks it needs a garbage collection based on Java heap size

-	Invoking finalize() method:

	-	Before removing an object from memory garbage collection thread invokes finalize() method of that object 
	-	And gives an opportunity to perform any sort of cleanup required
	-	We can also invoke this method on any instance in Java
	
		-	However, there is no guarantee that garbage collection will occur when you call this method
		-	finalize() method is declare and defined in java.lang.Object class with out body
		-	Sub-Class needs to provide an implementation for finalize() method for cleaning up resources if any
	

---------------------------------------------------------
## When does an object become eligible for garbage collection? Describe how the GC collects an eligible object?

-  	An object becomes eligible for Garbage collection or GC 
-	If it is not reachable from any live threads or by any static references

-	Straightforward:
	
	-	The most straightforward case of an object becoming eligible for garbage collection is if all its references are null

-	Cyclic dependencies: 
	
	-	Cyclic dependencies without any live external reference are also eligible for GC
	-	If object A references object B and object B references Object A 
	-	And they don’t have any other live reference 
	-	Then both Objects A and B will be eligible for Garbage collection

-	When a parent object is set to null:
	
	-	Another obvious case is when a parent object is set to null
	-	When a kitchen object internally references a fridge object and a sink object
	-	And the kitchen object is set to null
	-	Both fridge and sink will become eligible for garbage collection alongside their parent, kitchen

---------------------------------------------------------
## Suppose we have a circular reference (two objects that reference each other). Could such pair of objects become eligible for garbage collection and why?

-	Yes, a pair of objects with a circular reference can become eligible for garbage collection
-	This is because of how Java’s garbage collector handles circular references 
-	It considers objects live not when they have any reference to them
-	But when they are reachable by navigating the object graph starting from some garbage collection root (a local variable of a live thread or a static field)
-	If a pair of objects with a circular reference is not reachable from any root, it is considered eligible for garbage collection

---------------------------------------------------------

## What is generational garbage collection and what makes it a popular garbage collection approach?

-	Generational garbage collection is a strategy used by Garbage Collector 
-	Where the heap is divided into a number of sections called generations
-	Each sections will hold Objects based on their age
-	Whenever the garbage collector is running, the first step in the process is called marking
-	This is where the garbage collector identifies which pieces of memory are in use and which are not
-	This can be a very time-consuming process if all objects in a system must be scanned
-	As more and more objects are allocated, the list of objects grows and grows leading to longer and longer garbage collection time
-	With generational garbage collection, objects are grouped according to their “age” in terms of how many garbage collection cycles they have survived
-	This way, the bulk of the work spread across various minor and major collection cycles

---------------------------------------------------------
## Describe in detail how generational garbage collection works

### How Java heap is Structured?


-	The heap is divided up into smaller spaces or generations
	-	These spaces are:
		-	Young Generation
		-	Old or Tenured Generation
		-	Permanent Generation

#### Young Generation:
		
	- 	Young generation hosts most of the newly created objects
	-	Majority of objects are quickly short lived and therefore, soon become eligible for collection
	-	New objects start their journey here and are only “promoted” to the old generation space after they have attained a certain “age”
	-	The term “age” in generational garbage collection refers to the number of collection cycles the object has survived
	-	The young generation space is further divided into three spaces:
	
		-	An Eden space and two survivor spaces such as Survivor 1 (s0) and Survivor 2 (s1)

#### Old Generation:		
	
	-	Old generation hosts objects that have lived in memory longer than a certain “age”
	-	The objects that survived garbage collection from the young generation are promoted to this space
	-	It is generally larger than the young generation
	-	As it is bigger in size, the garbage collection is more expensive and occurs less frequently than in the young generation
	

#### Permanent Generation:
	
	-	The permanent generation or more commonly called, PermGen, contains metadata required by the JVM to describe the classes and methods -	 It also contains the string pool for storing interned strings
	-	In addition, platform library classes and methods may be stored here

	
### How Garbage Collector Works?

-	First, any new objects are allocated to the Eden space
-	Both survivor spaces start out empty
-	When the Eden space fills up:
	
	-	First Minor GC Cycle:
	
		-	A minor garbage collection is triggered
		-	Referenced objects are moved to the first survivor space(S0)
		-	Unreferenced objects are deleted
	
	-	Next Minor GC Cycle:
	
		-	During the next minor GC, the same thing happens to the Eden space
		-	Unreferenced objects are deleted and referenced objects are moved to a survivor space
		-	However, in this case, they are moved to the second survivor space (S1)
		-	In addition, objects from the last minor GC in the first survivor space (S0) have their age incremented and are moved to S1
		-	Once all surviving objects have been moved to S1, both S0 and Eden space are cleared
		- 	At this point, S1 contains objects with different ages
		
	-	Next Minor GC Cycle:

		-	At the next minor GC, the same process is repeated
		-	However this time the survivor spaces will switch
		- 	Referenced objects are moved to S0 from both Eden and S1
		- 	Surviving objects are aged
		-	Eden and S1 are cleared
	
	
-	After every minor garbage collection cycle:
 
	-	The age of each object is checked
	-	Those that have reached a certain arbitrary age, for example, 8, are promoted from the young generation to the old or tenured generation
	-	For all subsequent minor GC cycles, objects will continue to be promoted to the old generation space

- 	Major GC Cycle:
	
	-	This pretty much exhausts the process of garbage collection in the young generation
	-	Eventually, a major garbage collection will be performed on the old generation 
		
		-	Which cleans up and compacts that space
		-	For each major GC, there are several minor GCs
	
---------------------------------------------------------
## Is it possible to «resurrect» an object that became eligible for garbage collection?

-	Object Eligible for Garbage Collection:

	-	When an object becomes eligible for garbage collection, the GC has to run the finalize method on it
	-	The finalize method is guaranteed to run only once
	-	Thus the GC flags the object as finalized and gives it a rest until the next cycle

	
- 	finilize() method:
	
	-	In the finalize method we can technically “resurrect” an object, for example, by assigning it to a static field
	-	The object would become alive again and non-eligible for garbage collection, so the GC would not collect it during the next cycle

-	Deleting when not required:
	
	-	The object, however, would be marked as finalized
	-	So when it would become eligible again, the finalize method would not be called
	-	In essence, we can turn this “resurrection” trick only once for the lifetime of the object
	-	Beware that this ugly hack should be used only if we really know what you’re doing 
	— 	Understanding this trick gives some insight into how the GC works



---------------------------------------------------------
## Describe strong, weak, soft and phantom references and their role in garbage collection.

-	Java provides reference objects to control the relationship between objects and the garbage collector
-	By default, every object in a Java program is strongly referenced by a reference variable:

### Strong Reference:

	Code Snippet:
	
		StringBuilder sb = new StringBuilder();
		
-	In the above snippet, the new keyword creates a new StringBuilder object and stores it on the heap
-	The variable sb then stores a strong reference to this object
-	What this means for the garbage collector is that:
	
	-	The particular StringBuilder object is not eligible for collection at all due to a strong reference held to it by sb
	-	The story only changes when we nullify sb like this:

	Code Snippet:
	
		sb = null;
		

	-	After calling the above line, the object will then be eligible for collection

-	Changing Relationship:	
	
	-	We can change this relationship between the object and the garbage collector:
	- 	By explicitly wrapping it inside another reference object which is located inside java.lang.ref package

### Soft Reference:
	
	Code Snippet:
		
		StringBuilder sb = new StringBuilder();
		SoftReference<StringBuilder> sbRef = new SoftReference<>(sb);
		sb = null;

-	In the above snippet, we have created two references to the StringBuilder object
-	The first line creates a strong reference sb and the second line creates a soft reference sbRef
-	The third line should make the object eligible for collection but the garbage collector will postpone collecting it because of sbRef
-	When memory becomes tight and the JVM is on the brink of throwing an OutOfMemory error
-	In other words, objects with only soft references are collected as a last resort to recover memory

###	Weak Reference:

-	A weak reference can be created in a similar manner using WeakReference class
-	When sb is set to null and the StringBuilder object only has a weak reference
-	The JVM’s garbage collector will have absolutely no compromise and immediately collect the object at the very next cycle

###	Phantom Reference:

- 	Similar to Weak Reference
-	Object with Phantom Reference will be collected immediately
-	However, Phantom Reference are enqueued as soon as their objects are collected
-	We can poll the reference queue to know exactly when the objects was collected
	
---------------------------------------------------------
##  How are strings represented in memory?

-	A String instance in Java is an object with two fields: 
	
	-	A char[] value field 
	-	And an int hash field

		-	The "value" field is an array of chars representing the string itself
		-	And the "hash" field contains the hashCode of a string which is initialized with zero
		-	Calculated during the first hashCode() call and cached ever since
		-	If a hashCode of a string has a zero value, it has to be recalculated each time the hashCode() is called

-	String instance is immutable: we can’t get or modify the underlying char[] array
-	Another feature of strings is that the static constant strings are loaded and cached in a string pool
-	If we have multiple identical String objects in source code, they are all represented by a single instance at runtime

---------------------------------------------------------
##  What is a StringBuilder and what are its use cases? What is the difference between appending a string to a StringBuilder and concatenating two strings with a + operator? How does StringBuilder differ from StringBuffer?


###  String Builder

-	StringBuilder allows manipulating character sequences by appending, deleting and inserting characters and strings.
-	StringBuilder is a mutable data structure, as opposed to the String class which is immutable

###	String

-	When concatenating two String instances, a new object is created, and strings are copied
-	This could bring a huge garbage collector overhead if we need to create or modify a string in a loop
-	StringBuilder allows handling string manipulations much more efficiently

### String Buffer:

-	StringBuffer is different from StringBuilder in that it is thread-safe
-	If we need to manipulate a string in a single thread, use StringBuilder instead

---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##---------------------------------------------------------
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##
##




















