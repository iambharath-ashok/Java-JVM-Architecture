# Java Heap and Stack Area:  JVM Architecture


## Java Heap and Stack Area

### 1.	Stack Area

-	Stack Area is a area where method execution will happen
-	Each Method will get a Stack Frame for Execution
-	Latest Method will pushed to Stack on top of previous method 
-	Local variables will be allocated memory in the Stack frame 
-	Primitive variables will be allocated memory with in the Stack Area
-	Instance Variables are allocated Memory on the Heap and Reference to that Memory location will be assigned to Reference Variable in the Stack Area
- 	Once Method Execution gets completed will be removed from top of Stack
-	Method Stack Frame will be removed

### 2.	Heap Area

-	Heap Area is a Area where memory will be allocated for Instance or Objects
-	Reference to Instance or Object on Heap Area will be assigned to Reference Variables

-	Heap Area is divided into Young generation and Old generation 
-	Young Generation -> Where all the new objects are stored 
-	


##	JVM Architecture 

-	JVM responsibility is to load the class and run the class

### 1.	Compiler

-  	Java Compiler will compile the .java file and generates a .class file
-	.class contains a ByteCode representation of code 

---------------------------------------------------------

### 2.	Class Loading Subsystem

-	Class Loading Subsystem will take class from Hard Disk and load it to Main Memory
-	Loading is a process of Loading a class from Hard Disk to JVMs Method Area
-	.class file be loaded to JVM Method Area
-	JVM does this for every class in the Application
-	Once .class file is loaded in to Method Area, JVM will an create an Instance of that class of type java.lang.Class in the Heap Area
-	Its not actually an Instance of that class but a Class representation of that class - A class object of that class
-	Using that class instance we can access all the other information related to class
-	Class Loading Subsystem will verify the ByteCode and loads all the required classes to run the class or application



###	There are 3 types of Class Loaders

	#### 1.	Bootstrap Class Loader
	
	-	Bootstrap Class Loader is responsible for loading Internal Java Class Files from rt.jar
	-	rt.jar is having all the primary classes 
	-	Is responsible loading API classes that comes with JDK
	-	These Classes will be under JDK/JRE/lib/rt.jar
	-	Also know as Primordial Class Loader
	-	Written in Low level Language like C and C++	
		
	#### 2.	Extension Class Loader
	
	-	Extension Class Loader will load the classes from jre/lib/ext/...
	-	Extension Class Loader is responsible for loading extensible files or extra class files needs by JVM to run the application
	-	Loads class from JDK/JRE/lib/ext/security.jar ... many others
		
		
	#### 3.	Application Class Loaders
	
	-	Child of Extension Class Loader
	-	Application Class Loaders loads the classes from class path 
	-	Responsible for loading all Application Related Classes from ClassPath
	-	Application classes are third party jars added to application
	-	"-cp"  argument is used to pass as an argument
	-	If Application Class Loader is also not able to find the Class on ClassPath then ClassNotFoundException will be thrown


### Different Phases in Class Loading SubSystem

#### 1. Loading

-	Loading is a process of Loading a class from Hard Disk to JVMs Method Area
-	.class file will be loaded to JVM Method Area
-	JVM does this for every class in the Application
-	Once .class file is loaded in to Method Area, JVM will an Instance of that class of type java.lang.Class in the Heap Area
-	Its not actually an Instance of that class but a Class representation of that class - A class object of that class
-	Using that class instance we can access all the other information related to class


#### 2. Linking

-	Linking is divided into 3 sub activities


	### 1.	Verification
		
	-	Its a process of verifying that byte code of class is correct and not corrupted
	-	Byte Code Verifier will do this check
	-	If there are any error or misamatch in .class bytecode ... then JVM will throw java.lang.VerifyError
	
	### 2. Preparation
		
	-	In this process ... JVM will allocate memory for all the static, global variables and assigns default values for them
	-	All the class level variables and instance variables will allocated memory 
	-	All the instance variable or class level variables will be initialized to DEFAULT values
	-	int to 0, boolean to false, object to null
		
	### 3. Resolution or Resolving
		
	-	In this process ... reference will be replaced with actual memory location on the Method Area
	-	All the Class variables and Instance variables will be initialized with actual values
		
#### 3.	Initializing 

-	In this phases all the static variables are executed
-	All the static blocks will be executed from parent to child class ... top to bottom
-	If Class definition not found in the byte code then NoClassDefFoundError will be thrown

---------------------------------------------------------		
		
### 3.	Runtime Data Areas

#### 1. Method Area

-	When JVM came across the class it will check whether the class is already in the Method Area
-	If not ... it will ask Class Loader System to load the class to Method Area
-	All the .class files will be stored in Method Area
-	.class files will be loaded to JVM Method Area
-	When classes are loaded into memory ... all the binary information will be stored in Method Area
-	All the metadata about class will be stored in Method Area
-	Method Area is shared across all the threads
-	Started right from JVM Start Up
-	Method Area is also called PermGenSpace
-	Method Area size is 64MB by Default
-	Method Area can be tuned by XPermGen, Xss or Xms(maximum)
-	When millions of Classes gets loaded into Memory ... we may encounter OutOfMemoryException
-	In case of OutOfMemoryException ... we need to increase the PermGen Space
-	From Java 8, PermGenSpace is replaced by Metaspace
-	Metaspace is responsible for automatic memory allocation ... increasing and shrinking memory space automatically
-	Developer don't needs to specify the arguments for PermGenSpace 


#### 2.	Stack Area or Java Stack Area

-	Stack Area is a Area where method execution will happen
-	Each Method will get a Separate Stack Frame for Execution
-	Latest Method will pushed to Stack on top of previous method 
-	Local variables will be allocated memory in the Stack frame 
-	Primitive variables will be allocated memory with in the Stack Area
-	Instance Variables are allocated Memory on the Heap and Reference to that Memory location will be assigned to Reference Variable in the Stack Area
-	Once Method execution completes then Method Stack Frame will be removed
-	Once Thread execution completes whole Stack Area will removed by JVM
-	Each Method Stack Frame will have three 3 areas

		1.	Variable Array - All the method params and local variables will be stored
		2.	Operand Stack - Pushes and pops the statements in to operand stack
		3.	Frame Data - Points to Runtime Constant Pool and Exception Table

-	When JVM creates a Threads ... JVM will creates Stack for each Thread
-	Each Thread will gets its own Stack Area and one Thread can't access Stack of another Thread
-	All the method calls and local variables are stored in Stack Area
-	Due to inconsistent program may Stacks again ... again ... again resulting in StackOverFlowError

#### 3.	Heap Area

-	Heavily Used Memory Area
-	All the Objects of application are stored in Heap Area - Objects are allocated memory in Heap Area
-	By default Heap Memory will 1/4 of Physical Memory 
-	Heap Memory can be tuned up by Xs(Minimum) and Xms(Maximum)
-	All the Threads can and will share the Heap Area
-	Heap Area will be initialized at JVM Start up
-	JVM divides Heap Area into Young and Old Generations
-	Young Generation will have newly created objects and old objects will be moved to Old generation space

#### 4.	PC Registers

-	PC -> Program Counter Register
-	PC Register Area will always point to current(Next) executing statements
-	Each Thread will gets its own PC Registers Area
-	PC Register will point next instruction once current execution completed
-	PC is for Per Thread Management
-	There will be PC Register for each Thread

#### 5.	Native Method Stacks

-	OS dependent 
-	All Native OS Dependent classes will be loaded to this Stacks
-	.dll files in windows contains Native Class to interact with OS
-	This area is used to store the information to call the native languages

-----------------------------------------------------

### 4.	Execution Engine

-	Execution Engine is responsible for executing ByteCode Instructions
-	Execution Engine comprises of subsystems like JIT Compiler, Interpreter, Garbage Collector, Hotspot Profiler

-	Interpreter

	-	Interprets ByteCode instruction line by line 
	-	Checks whether ByteCode Instruction is compatible or not 

-	JUST In Compiler 

	-	Compiles repetitive code that needs to execute again and again to improve performance

-	Hotspot Profiler

	-	Helps JIT Compiler
	-	Provides statics to JIT Compiler that needs to be executed again and again 

----------------------------------------------------------

### 5.	Native Method Interface
### 6.	Native Method Libraries

----------------------------------------------------------
### 6.	Steps in Garbage Collector

####	Marking

-	Identify and mark what are all the objects that are not in use
	
####	Normal Deletion

-	Identified as unused will be removed 
	
####	Deletion and Compacting
	
-	Final Stage
-	Once objects are removed, memory allocation is random
-	Freed memory will be grouped together so that future memory allocation will be faster
-	Compacting - Grouping together available space together so that future memory allocation will be faster

-------------------------------------------------------------

### 5.Types of Garbage Collectors


####	Serial GC 
	
-	Useful when Mark and Delete objects
-	Suitable for small standalone applications
-	Small scale Applications

####	Parallel GC

-	Suitable for Large Scale Application
-	When Multiple Threads are running 
-	Uses N number of Threads Young Generation and only 1 Thread for Old Generation 

	
####	Parallel Old GC 

-	 Uses same number of Threads for both young generation and old generation	

####	Concurrent Mark Sweep

-	Does Garbage Collection for Old Generation only	

####	G1 Collector
	
-	Introduced in Java 8
-	Default in Java 9
-	Parallel and Young generation 


-----------------------------------------------------

## PermGen vs MetaSpace


### PermGen 

-	Method Area and it was inside Heap Area
-	Used to Store Objects of Methods and Classes
-	Used to Store Class Metadata
-	Apps with more Classes will have more PermGen Space
- 	May result in OOM due to lack of PermGen Space


### MetaSpace

-	Introduced from Java 8
-	Outside Heap and OS memory will be used
-	Process Size or Application Size can go large
-	Too many classes may bring down to Server


-----------------------------------------------------

##	Monitoring JVM

### JVisual VM

-	Can Monitor Multiple JVMs Running in the System
-	Can visualize Thread Dump, take Thread Dump Snapshot and Thread Status
-	Can visualize Heap Space and MetaSpace or PermGenSpace memory 
-	Memory Utilization



-----------------------------------------------------
