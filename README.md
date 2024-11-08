# DesignPatternNotes

Singleton Pattern
-
Concepts:
- Only one instance created
- Guarantees control of a resource
- Lazily loaded
- Examples
  - Runtime
  - Logger
  - Spring Beans
  - Graphics Managers

Design
- Class is responsible for lifecycle
- Static in nature
- Needs to be thread safe
- Private instance
- Private constructor
- No parameters required for construction

| Singleton                                    |
|----------------------------------------------|
| -singleton: Singleton                        |
| -Singleton()<br/> + getInstance(): Singleton |


Everyday Example - Runtime Env:
        
        Runtime singletonRuntime = Runtime.getRuntime();
        
        singletonRuntime.gc();

        System.out.println(singletonRuntime);

        Runtime anotherInstance = Runtime.getRuntime();

        System.out.println(anotherInstance);

        if(singletonRuntime == anotherInstance){
            System.out.println("They are the same instance");
        }

Pitfalls:
- Often overused
- Difficult to unit test
- if not careful, not thread-safe
- Sometimes confused for Factory
- Prototype (get a new unique instance)

Contrast

| Singleton                  | Factory                              |
|----------------------------|--------------------------------------|
| Returns same instance      | Returns various instances            |
| Once constructor - no args | Multiple constructors                |
| No interface               | Interface driven                     |
|                            | Adaptable to environment more easily |


- We want to use this pattern when we want to guarantee one instance
- Easy to implement
- Solves a well-defined problem
- Don't abuse it (not everything needs to be a singleton)


Builder Pattern
-
Concepts
- Handles complex constructors
- Large number of parameters
- Immutability
- Example:
  - StringBuilder
  - DocumentBuilder
  - Locale.Builder

Design:
- Flexibility over telescoping constructors
- Static inner class
- Call appropriate constructor
- Negates the need for exposed setters
- Java 1.5+ can take advantage of Generics

| Builder      |
|--------------|
|              |
| +buildPart() |

| ConcreteBuilder                |
|--------------------------------|
|                                |
| +buildPart()<br/> +getResult() |


Everyday Example - StringBuilder:

    StringBuilder builder = new StringBuilder();
    
    builder.append("This is an example");

    builder.append("of the builder pattern.");

    builder.append("It has methods to append");

    builder.append("almost anything we want to a String.");

    builder.append(42);

Pitfalls;
- Immutable
- Static Inner class
- Designed first 
- Complexity
- Method returns object


| Builder                      | Prototype                             |
|------------------------------|---------------------------------------|
| Handles complex constructors | Implemented around a clone            |
| No interface required        | Avoids calling complex constructors   |
| Can be a separate class      | Difficult to implement in legacy code |
| Works with legacy code       |                                       |


Prototype Pattern
-
The prototype.prototype pattern is used when the type of object to create is determined by a 
prototypical instance, which is cloned to produce a new instance.  
Oftentimes, the prototype.prototype pattern is used to get a unique instance of the same 
object over and over for each new request.

Concepts

- Avoids costly creation
- Avoids subclassing
- Typically, doesn't use "new"
- Often utilizes an Interface 
- Usually implemented with a Registry
- Example
  - java.lang.Object#clone()

Design

- Clone/Cloneable
- Avoids keyword "new"
- Although a copy, each instance unique
- Costly construction not handled by client
- Can still utilize constructor parameters
- Shallow VS Deep Copy

| <<interface>><br/>Prototype |
|-----------------------------|
| +Clone()<br/>+DeepCopy()    |


    Everyday Example - Statement 

    public class Statement implements Cloneable {
        public Statement(String sql, List<String> parameters, Record record) {
            this.sql = sql;
            this.parameters = parameters
            this.record = record;
          }

        public Statment clone() {
          try {
              return (Statement) super.clone();
          } catch (CloneNotSupportedException e) {}
            return null
        }
      }

Pitfalls

- Not always clear when to use
- Used with other patterns
- Registry
- Shallow VS Deep Copy


| Prototype                   | Factory               |
|-----------------------------|-----------------------|
| Lighter weight construction | Flexible Objects      |
| Copy Constructor or Clone   | Multiple constructors |
| Shallow or Deep             | Concrete Instance     |
| Copy of itself              | Fresh Instance        |


- Guaranteed unique instance
- often refactored in 
- can help w/ performance issues
- don't always jump to factory


Factory Method Pattern
-

Concepts

- Doesn't expose instantiation logic
- Defer to subclasses
- Common interface
- Specified by architecture, implemented by user
- Example
  - Calendar
  - ResourceBundle
  - NumberFormat

  Design

- Factory is responsible for lifecycle
- Common interface
- Concrete classes
- Parameterized create method

| Factory                  |
|--------------------------|
|                          |
| +factoryMethod(): Object |

<------

| ConcreteObject           |
|--------------------------|
|                          |
| +factoryMethod(): Object |


Everyday Example Calendar

    Calendar cal = Calendar.getInstance();

    System.out.println(cal);

    System.out.println(cal.get(Calendar.DAY_OF_MONTH));

Pitfalls:

- Complexity
- Creation in subclass
- Refactoring

Contrast

| Singleton                        | Factory                              |
|----------------------------------|--------------------------------------|
| Return same instance             | Returns various instances            |
| One constructor method - no args | Multiple constructors                |
| No interface                     | Interface driven                     |
| No Subclasses                    | Subclasses                           |
|                                  | Adaptable to environment more easily |

- Parameter driven
- Solves complex creation
- A little complex
- Opposite of a singleton


Abstract Factory Pattern
-

Concepts:

- Factory of Factories
- Factory of related objects
- Common interface
- Defer to Subclasses
- Example:
  - DocumentBuilder
  - Frameworks


Design

- Groups Factories together
- Factory is responsible for lifecycle
- Common interface
- Concrete classes
- Parameterized create method
- Composition


Everyday Example - DocumentBuilderFactory

    DocumentBuilderFactory abstractFactory =
    DocumentBuilderFactory.newInstance();

    DocumentBuilder factory =
    abstractFactory.newDocumentBuilder();

    Document doc = factory.parse(bias);


Pitfalls

- Complexity
- Runtime switch
- Pattern within a pattern
- Problem specific
- Starts as a Factory


Contrast

| Factory                              | AbstractFactory            |
|--------------------------------------|----------------------------|
| Returns various instances            | Implemented with a Factory |
| Multiple constructors                | Hides the Factory          |
| Interfaces driven                    | Abstracts Environment      |
| Adaptable to environment more easily | Built through composition  |


- Group of similar Factories
- Complex
- Heavy abstraction
- Framework pattern




# STRUCTURAL

Adapter Design Pattern
-

The adapter pattern is a great pattern for connecting new code to legacy code without having to 
change the working contract that was produced from the legacy code originally.


Concepts

- Plug Adapter
- Convert to another interface
- Legacy
- Translate requests
- Client, Adapter, Adaptee
- Examples:
  - Arrays -> Lists


Design

- Client centric
- Integrate new with old
- Interface, but not required
- Adaptee can be the implementation


| Client         |
|----------------|
|                |
| +doSomething() |


<--------

| <<interface>><br/>Adapter |
|---------------------------|
|                           |
| +doThis()                 |


<----------

| LegacyProduct |
|---------------|
|               |
| +doThat()     |


Everyday Example - Arrays.asList()


    Integer[] arrayOfInts = new Integer[] { 42, 43, 44};

    List<Integer> listOfInts = Arrays.asList(arrayOfInts);

    System.out.println(arrayOfInts);

    System.out.println(listOfInts);

Pitfalls:

- Not a lot
- Don't complicate
- Multiple adapters
- Don't add functionality

Contrast

| Adapter                      | Bridge                      |
|------------------------------|-----------------------------|
| Works after code is designed | Designed upfront            |
| Legacy                       | Abstraction/implementation  |
| Retrofitted                  | Built in advance            |
| Provides different interface | Both adapt multiple systems |

- Simple solution
- Easy to implement
- Integrate Legacy
- Can provide multiple adapters


Decorator Pattern
-
The decorator pattern is also a hierarchical type pattern that builds 
functionality at each level while using composition from similar data types.

Concepts

- Also called a wrapper
- Add behavior without affecting others
- More than just inheritance
- Single Responsibility Principle
- Compose behavior dynamically
- Example:
  - java.io.InputStream
  - java.util.Collections#checkedList


Design

- Inheritance based
- Utilizes composition and inheritance (is-a, has-a)
- Alternative to subclassing
- Constructor requires instance from hierarchy


Everyday Example - InputStream

    File file = new File("./output.txt");
    file.createNewFile();

    OutputStream oStream = new FileOutputStream(file);

    DataOutputStream doStream = new DataOutputStream(oStream);
    doStream.writeChars("text");


Pitfalls:

- New class for every feature added
- Multiple little objects
- Often confused with simple inheritance


Contrast

| Decorator                        | Composite                         |
|----------------------------------|-----------------------------------|
| Contains another entity          | Tree structure                    |
| Modifies behavior (adds)         | Leaf and Composite same interface |
| Doesn't change underlying object | Unity between objects             |



Facade Design Pattern
-
The façade pattern provides a simplified interface to a complex or difficult‑to‑use 
system that is often the result of a poorly‑designed API.

Concepts:

- Easier API usage
- Reduce external dependencies 
- Simplified interface
- Typically, refactoring pattern
- Examples:
  - java.net.URL


Design

- Class that utilizes composition
- Shouldn't need inheritance
- Typically encompasses full lifecycle


| Facade       |
|--------------|
| +doSomething |

<----------

| Package 1 |
|-----------|

| Package 2 |
|-----------|


Everyday Example - InputStream

    URL url = new URL("http", "www.pluralsight.com", 80,
      "/author/bryan-hansen");

    Buffered Reader in = new BufferedReader(
      new InputStreamReader(url.openStream()));

    String inputLine;

    while ((inputLine = in.reade.Line()) != null)  {
        System.out.println(inputLine);
    }

Pitfalls

- Typically used to clean up code
- Should think about API design
- Flat problem/structure
- The "Singleton" of Structural Patterns


Contrast

| Facade                | Adapter                        |
|-----------------------|--------------------------------|
| Simplifies Interface  | Also a refactoring pattern     |
| Works with composites | Modifies behavior (adds)       |
| Cleaner API           | Provides a different interface |

- Simplifies Client interface
- Easy pattern to implement
- Refactoring pattern



Proxy Design Pattern
-
The proxy pattern is a pattern that acts as an interface to something else, 
but it's often confused with just being a very simple pattern.
By nature, it's usually used with an interceptor to interact between two objects, 
and that's where the term proxy comes in.


Concepts:

- Interface by wrapping
- Can add functionality
- Security, Simplicity, Remote, Cost
- Proxy called to access real object
- Example:
  - java.lang.reflect.Proxy
  - java.rmi

Design

- Interfaced based 
- Interface and Implementation Class
- java.lang.reflect.InvocationHandler
- java.lang.reflect.Proxy
- Client, Interface, InvocationHandler, Proxy, Implementation


Everyday Example - Integer

    CustomService proxyService = (CustomerService)
    Proxy.newProxyInstance(
        customService.getClass().getClassLoader(),
        new Class[] {CustomService.class},
        customHandler);

    proxyService.doServiceCall();


Pitfalls:

- Only one Proxy
- Another Abstraction
- Similar to other patterns


Contrast

| Proxy                                           | Decorator                        |
|-------------------------------------------------|----------------------------------|
| Can add functionality, but not its main purpose | Dynamically add functionality    |
| Can only have one                               | Chained                          |
| Compile time                                    | Decorator points to its own type |
|                                                 | Runtime                          |

- Great utilities built into Java API
- Only one instance
- Used by DIJ/IoC Frameworks



Iterator Design Pattern
-
The iterator pattern is a great pattern for providing navigation 
without exposing the structure of an object.


Concepts

- Traverse a container
- Doesn't expose underlying structure
- Decouples algorithms
- Sequential
- Example:
  - java.util.Iterator
  - java.util.Enumeration

Design

- Interface based
- Factory Method based
- Independent, but fail fast
- Enumerators are fail safe
- Iterator, ConcreteIterator


Everyday Example - List

    List<String> names = new ArrayList<>();

    names.add("Bryan");
    names.add("Aaron");
    names.add("Jason");

    Iterator<String> namesItr = names.iterator();

    while(namesItr.hasNext()) {
        String name = nameItr.next();
        System.out.println(name);
        namesItr.remove();

    }



Pitfalls

- Access to Index
- Directional
- Speed / Efficiency


Contrast

| Iterator                | For Loop                         |
|-------------------------|----------------------------------|
| Interface based         | Traversal in client              |
| Algorithm is removed    | Exposes an index                 |
| No index                | Doesn't change underlying object |
| Concurrent modification | foreach syntax                   |
|                         | Typically slower                 |



- Efficient way to traverse
- Hides algorithm
- Simplify client
- foreach

Observer Design Pattern
- 
The observer pattern is a decoupling pattern when we have a subject that needs to be 
observed by one or more observers.


Concepts:

- One-to-Many
- Decoupled
- Event Handling
- Pub/Sub
- MVC
- Examples:
  - java.util.Observer
  - java.util.EventListener
  - javax.jms.Topic

Design

- Subject
- Observer
- Views are Observers
- Subject, Concrete Subject, Observer, Concrete Observer

Everyday Example - Observer

    TwitterStream messageStream = new TwitterStream();

    Client client1 = new Client("Bryan");

    Client client2 = new CLient("Mark");

    messageStream.addObserver(client1);
    messageStream.addObserver(client2);

    messageStream.someoneTweeted();

Pitfalls

- Unexpected updates
- Large sized consequences
- What changed
- Debugging difficult


Contrast

| Observer                | Mediator              |
|-------------------------|-----------------------|
| One-to-Many             | One-to-One-to-Many    |
| Decoupled               | Decoupled             |
| Broadcast Communication | Complex Communication |


- Decoupled communication
- Builtin functionality
- Used with mediator


Strategy Design Pattern
-

The strategy pattern is a behavioral pattern that is used when 
you want to enable the strategy or algorithm to be selected at runtime.

Concepts:

- Eliminate conditional statements
- Behavior encapsulated in class
- Difficult to add new strategies
- Client aware of strategies Client chooses strategy
- Examples:
  - java.util.Comparator

Design:

- Abstract base class
- Concrete class per strategy
- Remove if/else conditionals
- Strategies are independent
- Context, Strategy, ConcreteStrategy


Everyday Example - Comparator

    Collections.sort(people, new Comparator<Person>() {
       @Override
        public int compare(Person o1, Person o2) {
          if(o1.getAge() > o2.getAge()) {
            return 1;
        }

        if(o1.getAge() < o2.getAge()) {
          return -1
        }

      return 0;

        }
    }};

Pitfalls

- Client aware of Strategies
- Increased number of classes

Contrast


| Strategy                     | Statue          |
|------------------------------|-----------------|
| Interface based              | Interface based |
| Algorithms are Independent   | Transitions     |
| Class per Algorithm          | Class per State |


- Externalizes algorithms 
- Client knows different Strategies
- Class per Strategy
- Reduce conditional statements


State Design Pattern
-
The state pattern is used when we need to represent state in an application.

Concepts

- Localize state behavior
- State Object
- Separates What from Where
- OCP
- Examples:
  - None!
  - JSF
  - Iterator

Design

- Abstract Class/ Interface
- Class based
- Context unaware
- Context, State, ConcreteState


Everyday Example - if/else

    final static int ON = 0;
    final static int OFF = 1;
    int state = OFF;

    public void pullChain() {
      if(state == ON) {
          System.out.println("Fan is already on");
      }
      else if (state == OFF) {
        System.out.println("Turning Fan on.");
        state = ON;
      }
    }

Pitfalls

- Know your States
- More classes
- Keep logic out of Context 
- State change triggers

Contrast


| State           | Strategy                   |
|-----------------|----------------------------|
| Interface based | Interface based            |
| Transitions     | Algorithms are Independent |
| Class per State | Class per Algorithm        |


- Simplifies cyclomatic complexity
- Adding additional states made easier
- More classes
- Similar implementation to Strategy


Command Design Pattern
-

The command pattern is a behavioral pattern that lets you 
encapsulate each request as an object.

Concepts

- Encapsulate request as an Object
- Object-oriented callback
- Decouple sender from processor
- Often used for "undo" functionality
- Examples:
  - java.lang.Runnable
  - javax.swing.Action

Design

- Object per command 
- Command interface
- Execute Method
- 'Unexecute' method
- Reflection
- Command, Invoker, ConcreteCommand


Everyday Example - Runnable

    Task task1 = new Task(10, 12);
    Task task2 = new Task(11, 13);

    Thread thread1 = new Thread(task1);
    thread1.start();

    Thread thread2 = new Thread(task2);
    thread2.start();

Pitfalls

- Dependence on other patterns
- Multiple Commands
- Make use of Memento
- Prototype for copies


Contrast

| Command                   | Strategy                 |
|---------------------------|--------------------------|
| Object per command        | Object per strategy      |
| Class contains the 'what' | Class contains the 'how' |
| Encapsulation action      | Encapsulation algorithm  |

- Encapsulation each request as an object
- Decouple sender from processor
- Very few drawbacks
- Often used for undo functionality






