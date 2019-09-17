# Static keyword in C++ Vs Java

## Static Data Members:

Like C++, static data members in Java are class members and shared among all objects. For example, in the following Java program, static variable count is used to count the number of objects created.


```java
class Test { 
    static int count = 0; 
  
    Test() {  
       count++;  
    }     
    public static void main(String arr[]) { 
       Test t1 = new Test(); 
       Test t2 = new Test(); 
       System.out.println("Total " + count + " objects created");         
    } 
} 
```
**Output:** Total 2 objects created
********


## Static Member Methods:

Like C++, methods declared as static are class members and have following restrictions:

1)They can only call other static methods. 
For example, the following program fails in compilation. fun() is non-static and it is called in static main()

``` java

		class Main { 
			public static void main(String args[]) {    
				System.out.println(fun()); 
			}  
			int fun() { 
				return 20; 
			}  
		} 
```

2)They must only access static data.

3)They cannot access this or super . For example, the following program fails in compilation.
```java
class Derived extends Base  
{ 
   public static void fun() { 
       System.out.println(super.x); // Compiler Error: non-static variable  
                                  // cannot be referenced from a static context 
   }    
} 
```

Like C++, static data members and static methods can be accessed without creating an object. They can be accessed using class name. For example, in the following program, static data member count and static method fun() are accessed without any object.

```java
class Test { 
    static int count = 0; 
    public static void fun() {  
       System.out.println("Static fun() called");  
    }     
}    
       
class Main 
{ 
    public static void main(String arr[]) { 
       System.out.println("Test.count = " + Test.count);         
       Test.fun(); 
    } 
}  
```

## Static Block: 
<b>Unlike C++, Java supports a special block, called static block (also called static clause) which can be used for static initialization of a class.</b> The code inside static block is executed only once. See Static blocks in Java for details.

## Static Local Variables:
Unlike C++, Java doesn’t support static local variables. For example, the following Java program fails in compilation.

```java
class Test { 
   public static void main(String args[]) {  
     System.out.println(fun()); 
   }  
   static int fun()  { 
     static int x= 10; //Compiler Error: Static local variables are not allowed 
     return x--; 
   } 
}  

```

# Static and non static blank final variables in Java

A variable provides us with named storage that our programs can manipulate. There are <b>two types of data variables in a class:</b>

**Instance variables :** 
Instance variables are declared in a class, but outside a method, constructor or any block. When a space is allocated for an object in the heap, a slot for each instance variable value is created. Instance variables are created when an object is created with the use of the keyword ‘new’ and destroyed when the object is destroyed. They are property of an object so they can be accessed using object only.

**Static variables :** Class variables also known as static variables are declared with the static keyword in a class, but outside a method, constructor or a block. There would only be one copy of each class variable per class, regardless of how many objects are created from it. They are property of a class not of an object so they can be used directly using class name as well as using object.

```java
// Java code to illustrate use of instance and static variables 
public class Emp { 
    String name; 
    int salary; 
    static String company; 
    public void printDetails() 
    { 
        System.out.println("Name: " + name); 
        System.out.println("Company: " + company); 
        System.out.println("Salary: " + salary); 
    } 
    public static void main(String s[]) 
    { 
        Emp.company = "GeeksForGeeks"; 
        Emp g = new Emp(); 
        g.name = "Shubham"; 
        g.salary = 100000; 
  
        Emp sp = new Emp(); 
        sp.name = "Chirag"; 
        sp.salary = 200000; 
  
        g.printDetails(); 
        sp.printDetails(); 
  
        g.company = "Google"; 
        g.salary = 200000; 
        System.out.println("\nAfter change\n"); 
        g.printDetails(); 
        sp.printDetails(); 
    } 
} 
```
**Output:**
```
Name   : Shubham
Company: GeeksForGeeks
Salary : 100000
Name   : Chirag
Company: GeeksForGeeks
Salary : 200000

After change
Name   : Shubham
Company: Google
Salary : 200000
Name   : Chirag
Company: Google
Salary : 200000
```
In the above example, by changing the company name it is reflected in all other objects as it is a static variable. But changing the salary of g doesn’t change the salary of s because salary is an instance variable.

**Blank final variable :** A final variable declared but not assigned is known as a blank final variable. It can be initialized within a constructor only. It raises compilation error if it is not initialized because it should be given a value somewhere in the program and that too from a constructor only.

## Static blank final variable :
It is a blank final variable declared as static. That is, a final static variable declared but not given a value or not initialized is known as static blank final variable.It can be initialized through a static block only.
Here is an example illustrating initialization of blank final variables-

```java
// Java program to illustrate initialization  
// of blank final variables 
public class GFG { 
    private static final int a; 
    private final int b; 
    static
    { 
        a = 1; 
    } 
    GFG(int c) 
    { 
        b = c; 
    } 
    public static void main(String s[]) 
    { 
        GFG g1 = new GFG(10); 
        GFG g2 = new GFG(20); 
        System.out.println(GFG.a); 
        System.out.println(g1.b); 
        System.out.println(g1.b); 
    } 
} 
```
**Output:**
```
1
10
10
```
In the above example, b is initialized using constructor while a using static block.
Predict the output of the following program :

```java
// Java program to illustrate  
// static blank final variable 
public class UserLogin { 
    public static final long GUEST_ID = -1; 
    private static final long USER_ID; 
    static
    { 
        try { 
            USER_ID = getID(); 
        } 
        catch (IdNotFound e) { 
            USER_ID = GUEST_ID; 
            System.out.println("Logging in as guest"); 
        } 
    } 
    private static long getID() 
        throws IdNotFound 
    { 
        throw new IdNotFound(); 
    } 
    public static void main(String[] args) 
    { 
        System.out.println("User ID: " + USER_ID); 
    } 
} 
class IdNotFound extends Exception { 
    IdNotFound() {} 
} 
```
**Output:**
`
prog.java:8: error: variable USER_ID might already have been assigned
USER_ID = GUEST_ID;
^
1 error
`
The USER_ID field is a static blank final. It is clear that the exception can be thrown in the try block only if the assignment to USER_ID fails, so it is perfectly safe to assign to USER_ID in the catch block. Any execution of the static initializer block will cause exactly one assignment to USER_ID, which is just what is required for blank finals. But this program fails because, A blank final field can be assigned only at points in the program where it is definitely unassigned.

Here, compiler is not sure whether its been assigned in try block or not, so the program doesn’t compile. We can solve this by removing the static block and initializing the USER_ID at the time of declaration.

``` java
// Java program to illustrate  
// static blank final variable 
public class UserLogin { 
    public static final long GUEST_ID = -1; 
    private static final long USER_ID = getUserIdOrGuest(); 
    private static long getUserIdOrGuest() 
    { 
        try { 
            return getID(); 
        } 
        catch (IdNotFound e) { 
            System.out.println("Logging in as guest"); 
            return GUEST_ID; 
        } 
    } 
    private static long getID() 
        throws IdNotFound 
    { 
        throw new IdNotFound(); 
    } 
    public static void main(String[] args) 
    { 
        System.out.println("User ID: " + USER_ID); 
    } 
} 
class IdNotFound extends Exception { 
    IdNotFound() {} 
} 
```
**Output:**
`
Logging in as guest
User ID: -1
`
***********



























| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

| Left-aligned | Center-aligned | Right-aligned |
| :---         |     :---:      |          ---: |
| git status   | git status     | git status    |
| git diff     | git diff       | git diff      |

