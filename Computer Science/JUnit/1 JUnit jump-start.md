>Never in the field of software development was so much owed by so many to so few lines of code. -Martin Fowler

# 1.1 Proving that a program works
- Automated tests are essential parts of the development process: you cannot *prove* that a component works until it passes a comprehensive series of tests. 
- Erich Gamma, one of the authors of [[Design Patterns]], and Kent Beck, the author of [[Extreme Programming Explained - Embrace Change]], created a simple but effective unit testing framework for Java called [[JUnit 5|JUnit]] in 1997.
	- *Framework* - A semi-complete application that provides a reusable common structure to share among applications. Developers incorporate the framework into their own applications and extend it to meet their needs. Frameworks differ from tool kits or libraries by providing a coherent structure rather than a simple set of utility classes.  

---

- *Unit test* - A test that examines the behavior of a distinct unit of work. A unit of work is a task that is not directly dependent on the completion of any other task. Within a Java application, the distinct unit of work is often, but not always, a single method. 
	- In contrast, *integration tests* and *acceptance tests* examine how various components interact.  
- Unit tests often focus on testing whether a method is following the terms of its API contract. An API contract is a formal agreement made by the signature of a method. A method requires its caller to provide specific object references or primitive values and returns an object reference or primitive value. If the method cannot fulfill the contract, a test should throw an exception.
	- *API contract* - A view of an API as a formal agreement between the caller and callee. Often unit tests help define the API contract by demonstrating the expected behavior.  

# 1.2 Starting from scratch
- Our first example is a very simple *Calculator* class that adds two numbers. It provides an API to clients and does not contain a user interface. To test its functionality we'll first create our own pure Java tests and later move to JUnit 5.  

```Java
public class Calculator {
	public double add(double number1, double number2) {
		return number1 + number2;
	}
}
```

- The compiler can tell you that the code compiles, but you should also make sure it works at runtime. A core principle of unit testing is, "Any program feature without an automated test simply doesn't exist" - Kent Beck. The *add* method represents a core feature of the calculator. You have some code that allegedly implements the feature. What is missing is an automated test that proves the implementation works.  

- When testing what we really want to know is whether the method works when it ships with the rest of the application or whenever you make a subsequent modification. If we put these requirements together we come up with the idea of writing a simple test program.
	- The test program can pass known values to the method and see whether the result matches expectations. You can also run the program again later to be sure the method continue to work as the application grows.

```Java
public class CalculatorTest {

	private int nbErrors = 0;

	public void testAdd() {
		Calculator calculator = new Calculator();
		double result = calculator.add(10, 50);
		if(result != 60) {
			throw new IllegalStateException("Bad result: " + result);
		}
	}

	public static void main(String[] args) {
		CalculatorTest test = new CalculatorTest();
		try {
			test.testAdd();
		}
		catch(Throwable e) {
			test.nbErrors++;
			e.printStackTrace();
		}
		if(test.nbErrors > 0) {
			throw new IllegalStateException("There were " + test.nbErrors + " error(s)");
		}
	}
}
```

- With the above setup additional methods can be added with more unit tests later without making the *main* method harder to maintain and the *main* method prints a stack trace when an error occurs along with a summary exception.  

- With this simple example you can see that even this small class and its tests can benefit from a little bit of skeleton code to run and manage test results. But as an application gets more complicated and tests become more involved, continuing to build and maintain a custom testing framework becomes a burden.  

## 1.2.1 Understanding unit testing frameworks
- Unit testing has several best practices that frameworks should follow:
	1. Each unit test should run independently of all other unit tests.
	2. The framework should detect and report errors test by test.
	3. It should be easy to define which unit tests will run.  

## 1.2.2 Adding unit tests
- To add tests to the above *CalculatorTest* class you would add a new test method and then add the corresponding *try/catch* block to *main*. This is short of what we would want in a real test suite and large *try-catch* blocks cause maintenance problems.
- It would be nice if you could just add new test methods and continue working but how would the program know which methods to run? 
	- You could have a simple registration procedure. A registration method would at least inventory which tests are running.
	- Another approach would be to use Java's reflection capabilities. A program could look at itself and decide to run whatever methods follow a certain naming convention, such as those that begin with *test*.  

- The support code that make it easy to add new tests (via registration or reflection) would not be trivial, but it would be worthwhile. You'd have to do a lot of work upfront but that effort would pay off each time you add a new test.  

- Fortunately the JUnit framework already supports discovering methods. It also supports using a different class instance and class loader instance for each test and reports all errors on a test-by-test basis. The JUnit team defined three discrete goals for the framework:
	1. The framework must help us write useful tests.
	2. The framework must help us create tests that retain their value over time.
	3. The framework must help us lower the cost of writing tests by reusing code.

# 1.3 Setting up JUnit
- To use JUnit you need to know about its dependencies. JUnit 5 is a modular framework; you can no longer simply add a jar file to your project compilation classpath and your execution classpath. In fact the architecture of JUnit 5 is no longer monolithic, discussed further in [[3 JUnit architecture|JUnit Architecture]]. 
- To manage JUnit 5's dependencies efficiently it's logical to work with a build tool, in this case we'll use Maven. See [[10 Running JUnit tests from Maven 3|Running JUnit tests]] for more info. 
	- The dependencies that are always needed in the pom.xml file are shown in the following listing:

```xml
<dependency>
	<groupId>org.junit.jupiter</groupId>
	<artifactId>junit-jupiter-api</artifactId>
	<version>5.6.0</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>org.junit.jupiter</groupId>
	<artifactId>junit-jupiter-engine</artifactId>
	<version>5.6.0</version>
	<scope>test</scope>
</dependency>
```

