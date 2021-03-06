# OCP Java SE 11 Programmer I Summary

1Z0-815

## Java Building Blocks

### Terminology

| Terminology | Meaning |
|:---|:---|
|*object* | runtime instance of a class in memory
|*members* | *fields* + *methods*
|*keyword* | word with special meaning (e.g. `public`, `class`)
|*access modifier* | declare level of exposure (e.g. `public`, `private`)
|*parameter* | sent into method
|*method signature* | method name and parameter list
|*code block* | code between braces (`{code}`)
|*instance initializer* | code blocks that appear outside methods
|*numeric literal* | a number that occurs in code
|*variable* | name of a piece of memory that stores data
|*local variable* | defined inside a method
|*instance variable* | field
|*class variable* | variable with static keyword in initialization

### Java keywords

| keywords |||||
|--|--|--|--|--|
| abstract | assert | boolean | break | byte |
| case | catch | char | class | const |
| continue | default | do | double | else |
| enum | extends | false | final | finally |
| float | for | goto | if | implements |
| import | instanceof | int | interface | long |
| native | new | null | package | private |
| protected | public | return | short | static |
| strictfp | super | switch | synchronized | this |
| throw | throws | transient | true | try |
| void | volatile | while |

### Comments

- single-line comment (`//`)
- multiple-line comment (`/* ... */`)
- Javadoc comment (`/** ... */`)

### Running and Compiling

Java files are compiles with `javac` command. `java` is used to run a *.class* file.

- `-d` flag can be used to put generated *.class* files in a target directory.
- `-cp`, `-classpath` or `--class-path` can be used to specify the classpath.

#### JAR files

A **Java archive** (JAR) is like a zip file containing mainly *.class* files.

To **include** *.jar* files, they should be included in the class path, e.g.

```sh
java -cp ".:/tmp/someLocation/*:/tmp/someOtherLocation:/tmp/myJar.jar" myPackage.myClass
```

To **create** *.jar* files, use the `jar` command.

The `jar` command has several flags

| flag ||
|:--|:--|
| `-c` or `--create` | creates a new JAR file |
| `-v` or `--verbose` | |
| `-f <fileName>` or `--file <fileName>` | JAR filename |
| `-C <directory>` | directory containing files to be used to create JAR |

#### single-file source-code

A program can be compiled and run directly with the `java` command if

- it is entirely contained in **one** file, and
- it does not use any non-JDK *.class* files.

#### Files

- For multiple classes in the same file, at most **one** is allowed to be public.
- A public class needs to match the filename.
- When a file is compiled, multiple `.class` files will be created, one for each
class.
- The compiled class will be named *after the class*, not the file.
- A file must have `.java` extension in order to be executed.
- Java class files run on the JVM.

### main()

- A Java program begins execution with its `main()` method. It is the gateway
between the startup of a Java process (handled by JVM), and the beginning of a
programmer's code.
- All command-line arguments are treated as *String* objects.

### Packages

- Java put classes in *packages*.
- Import statements tell Java which packages to look in for classes.
- packages beginning with java or javax comes with the JDK
- Naming rules are the same as for variable names.
- Wildcard imports `.*` imports all classes in package, but not in child packages.
- Only one wildcard can be used, and it must be used at the end of the line
( e.g. neither `java.nio.*.*` nor `java.nio.*.Paths` works)
- `java.lang` is automatically imported.
- Java automatically looks in the current package for other classes.
- A compilation unit without package is considered an _unnamed package_, they
cannot be reached from other packages.

#### Naming conflicts

If naming conflicts occur, explicit import takes precedence over wildcard import:

```java
java.util.Date
java.sql.* //java.sql.Date is omitted
```

Import ambiguity is ok, but when ambiguity occurs when referencing it result in
compiler error.

```java
java.util.*
java.sql.* // OK
Date date = new Date(); // Compiler error
```

If both needs to be used, the fully qualified name can be used.

##### Compilation

For two files `packagea/ClassA.java` and `packageb/ClassB.java`, *ClassB* can
import *ClassA* by the statement `import packagea.ClassA`.
Compilation will create a class file in respective directory.
`java packageb.ClassB` will run ClassB from `.` directory.

Location of other files can also be explicitly specified using a class path. For
example in JAR files (JARs are like zip files containing mostly Java class files).

### Primitive Types, Reference Types and Literals

#### Primitive types

| Keyword | Type |
| :--- | :--- |
| `boolean` | true or false |
| `byte` | 8-bit integral value |
| `short` | 16-bit integral value (signed) |
| `int` | 32-bit integral value |
| `long` | 64-bit integral value |
| `float` | 32-bit floating-point value |
| `double` | 64-bit floating-point value |
| `char` | 16-bit Unicode value (unsigned)|

- Primitive types defaults to 0 in their respective type(except for local variables).
- Even `char` is promoted to `int`.

To calculate the range of a primitive type, take 2 to the power of bits and
divide by 2 to account for negative numbers, and subtract 1 to account for 0
(which is taken from positive numbers). E.g a `byte` can hold a value from -128
to 127. 1 `byte` = 8 `bits`.:

```math
max(1 byte) = 2^8/2 - 1 = 127.
```

#### Literals

- When a literal *integer* occurs, Java assumes it is an `int` by default. E.g.

```java
long max = 3123456789; // Does not compile because literal is too large for int
long max = 3123456789L; // Compiles
```

- When a *floating-point* literal occurs, Java assumes it is a `double` by
default. E.g.

```java
float x = 2.1; // Does not compile
float x = 2.1f; // Compiles
```

**Note**: `short s = 1;` compiles while `float f = 1.0;` does not.

Java accepts other number formats:

|Name|Range|Prefix|Example|
|:--|:--|:--|:--|
|octal|[0-7]| **0**|`017`|
|hexadecimal| [0-9, A-F]| **0x** or **0X** |`0xFF`|
|binary| [0-1]| **0b** or **0B** |`0b10`|

- Java allows underscores in literals, but not in the beginning, end or next to
a decimal point.

```java
double thousand = 1_00_0.0_0; // Compiles
```

##### The widening primitive conversions

- *The widening primitive conversions* states that primitives can be converted
to a larger primitive: (`byte` or `char`) -> `short` -> `int` -> `long` -> `float` -> `double` e.g.

```java
int amount = 0b101;
int amount = 0xE;
double amount = 0xE;
```

- Despite the fact that loss of precision may occur, a widening primitive
conversion never results in a run-time exception.

- For conversion in the other direction, explicit cast is required.

#### Reference Types

A *reference type* refers to an object. A reference points to an object by
storing the memory address where the object is located.
A value can be assigned to a reference in one of two ways

- with the `new` keyword
- another object of the same type

#### Differences

- Reference types can be assigned `null`. Primitive types will give compiler
error if null is assigned.
- Reference types can be used to call methods (e.g. `myCat.getAge()`). Primitive
types does not have methods.
- All primitives use lowercase type names (it is convention to begin class names
with an uppercase letter).

### Declaring Variables

Multiple variables can be declared and initialized in the same statement.
Many variables can be **declared** in the same statement, provided they are of
the same type (however, type is only allowed to be declared once for each
statement). Any, or all of them can be **initialized** inline.

**Note**: It is not allowed to *only* initialize multiple variables in the same
statement. E.g.

```java
int x;
int y;
x = 1, y = 1; // Does not compile
```

### `var`

- Used as local variable.
- Cannot be used in constructor parameters, method parameters, instance variables, or class variables
- Always initialized on the same line as it is declared.
- Value can change, but not type.
- Cannot be initialized with a `null` value without a type.
- `var` is not allowed in a multiple-variable declaration.
- `var` is a reserved type name, but not a reserved word. Meaning it cannot be used to define a type.
(E.g `var var = "string";` compiles).

### Identifiers

These three rules apply to everything the programmer is free to name (e.g.
methods, classes, variables)

- identifier must begin with a *letter*, *$* or *_*
- subsequent characters can be numbers
- Java reserved words cannot be used (however, Java is case sensitive)

### Default Initialization & Scope

- *Local variables* are variables defined within a method and can never have a
larger scope than the method they are defined in. In scope from declaration to
end of block.
- *Instance variables* are also known as fields. In scope from declaration until
object is garbage collected.
- *Class variables* are variables with the `static` keyword. In scope from
declaration until program ends.

**Note**: Local variables must be initialized before used. Instance variables
and class variables are assigned a default value:

| Variable type | Default initialization value |
|:---|:---|
| boolean | `false` |
| byte, short, int, long | `0` (in the type's bit-length) |
| float, double | `0.0` (in the type's bit-length) |
| char | `'\u0000'` (NUL) |
| All object references | `null` |

### Order of Elements in Class

| Element | Example | Required | Where does it go? |
| :--- | :--- | :---: | :--- |
| Package declaration | `package abc;` | No | First line in file |
| Import statements | `import java.util.*` | No | Immediately after the package |
| Class declaration | `public class C` | Yes | Immediately after import |
| Field declarations | `int value;` | No | Anywhere inside a class |
| Method declarations | `void method()` | No | Anywhere inside a class |

**Note**: comments can go anywhere, even on first line before package.

### Destroying Objects

All Java objects are stored in the program's memory *heap* (*free store*);
memory allocated to the Java program.

`System.gc()` is not guaranteed to run. It might be ignored by Java.

An object will remain on the heap until it is no longer reachable. An object is
no longer reachable if anyone of the following occurs:

- The object no longer has any references pointing to it.
- All references to the object has gone out of scope.

#### finalize()

Objects may implement `finalize()` which gets called when the garbage collector
tries to collect the object. But if the garbage collector fails the first time,
it won't call `finalize()` the second time. All in all, there is no guarantee that
finalize will run, and it wont run more than once.

`finalize()` was deprecated in Java 9.

### Benefits of Java

|||
|:---|:---|
| **Object oriented** | All code is defined in classes
| **Encapsulation** | Java supports access modifiers
| **Platform independent** | Java gets compiled to bytecode "Write once, run everywhere".
| **Robust** | Prevents memory leaks.
| **Simple** | Pointers and operator overloading eliminated (compared to C++).
| **Secure** | Java code runs inside the JVM, so it is sandboxed.

## Operators and Statements

| Terminology | Meaning |
|:---|:---|
|*operator* | special symbol that can be applied to operands and returns a result
|*operand* | variable, value or literal operated on by an operator
|*assignment operator* | binary operator that modifies, or assigns, the variable on the left hand side of the operator with the result of the value of the right-hand side
|*logical operator* | `&`, `\|`, `^`
|*bitwise operator* | logical operators applied to numerical data types

### Operators

Three types: **unary**, **binary**, **ternary**.

Order of operation (by decreasing order of operator precedence)

**Note**: Not to be confused with *evaluation order*. Before applying any _operator_,
all _operands_ must be evaluated left-to-right. There is no _operand_ order of precedence
other than left-to-right.

| Operator | Symbols and examples |
|:---|:---|
| **unary**
| post-unary operators | `expression++`, `expression--`
| pre-unary operators | `++expression`, `--expression`
| other unary operators | `~`, `+`, `-`, `!`, `(type)`
| **binary**
| multiplication/division/modulus | `*`, `/`, `%`
| addition/subtraction | `+`, `-`
| shift operators | `<<`, `>>`, `>>>`
| relational operators | `<`, `>`, `<=`, `>=`, `instanceof`
| equal to/not equal to | `==`, `!=`
| logical operators | `&`, `^`, `\|`|
| short-circuit logical operators | `&&`, `\|\|`|
| **ternary**
| ternary operators | `boolean expression ? expression1 : expression2`
| **assignment**
| assignment operators | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `^=`, `\|=`, `<<=`, `>>=`, `>>>=`

**Note**: If two operators have the same level of precedence, Java guarantees
**left-to-right evaluation**. Except for assignment operators which evaluates
from **right-to-left**.

**Note**: Order of precedence can be overridden by parenthesis.

#### Binary Operators

Two operands.

##### Arithmetic Operators

- Includes `+`, `-`, `*`, `/`, `%` (they also include the unary operators `++`
and `--` ).
- All arithmetic operators can be applied to all primitive types except `boolean`
and `String`. For `String`, only the operators `+` and `+=` can be applied
(String concatenation).
- For integers, division result in the floor value.

##### Numeric Promotion

Applies for **binary arithmetic** operators.

Numeric promotion rules:

1. If two values have different data types, Java will automatically promote the
lesser to the larger of the two types.
2. If one value is integral and the other is floating-point, Java will
automatically promote the integral value to the floating-point's data type.
3. Smaller data types (`byte` , `short`, and `char`) are first promoted to
`int` any time they are used in a **binary** arithmetic operator (even if neither of
them are `int`).
4. After all promotion has occurred and the operands have the same data type,
the resulting value will have the same data type as its promoted operands.

**Note**: Rules only applies for **binary** operators, meaning that the following
example is perfectly legal

```java
short x = 1;
short y = ++x; // COMPILES
```
However, the example below break rule 3

```java
short x = 1; // compile-time narrowing
short y = 1; // compile-time narrowing
short z = x*y; // DOES NOT COMPILE
```

**Note**: Compound assignment will automatically cast value to the variable's type.

#### Unary Operators

One operand. `+`, `-`, `++`, `--`, `!`, `~`.

On the exam, look out for questions that mixes logical and numerical negation, e.g.

```java
int x = !5; // Does not compile
boolean y = -true; // Does not compile
boolean z = !0; // Does not compile
```

**Note**: in Java *0* and *false*, *1* and *true* are not related in any way.

For pre-unary operators, the value is decremented *before* it is returned. For
post-unary operators, the value is incremented *after* it is returned. E.g.

```java
int x = 0;
System.out.println(++x); // Output: 1
System.out.println(x--); // Output: 1
System.out.println(x); // Output: 0
```

```java
int x = 3;
int y = ++x * 5 / x-- + --x; // Results in: x = 2, y = 7
```

#### Ternary Operator

There is only one in java, namely `? :`, `booleanExpression ? expression1 : expression2;`

- The first operand must be a boolean expression, second and third operands can be
any expression that returns a value.
- It is not required that second and third operand is of the same type, although
it may come in to play when combined with the assignment operator.

**Note**: Only one of the right hand side operators will be evaluated at
runtime, so side-effects might not be applied at runtime.

#### Casting Primitive Values

Casting is required when you are going from a larger numerical data type to a
smaller numerical data type, or converting from floating-point to integral value.

```java
int x = (int) 1.0;
short y = (short) 1921222; // Stored as 20678 (numeric overflow)
```

Numeric *overflow* is when the number is too large to be stored in the data
type. There is also an *underflow* when the number is too low to fit in the data
type.

**Note**: Compound assignment operators will *automatically* cast the resulting
value into the type of the left-hand side, e.g.

```java
long x = 10;
int y = 5;
y = y * x; // Does not compile
y *= x; // Compiles
```

The result of the (compound) assignment operator, `=`, is an expression of
itself, i.e.

```java
long x = 5;
long y = (x=3); // Gives: x = 3, y = 3
```

#### Relational Operators

Relational operators compare two expressions and return a boolean value.

`<`, `>`, `<=`, `>=` are applied to numeric primitive data types only. If two
numeric data types are different, numeric promotion takes place.

The `instanceof` operator, e.g. `a instanceof b` is true if the reference that a
points to is an instance of a class, subclass or class that implements a
particular interface, as named in b.

#### Logical Operators

AND `&`, inclusive OR `|`, Exclusive OR `^`.

Can be applied to both numeric and boolean data types. When they are applied to
boolean data types, they are referred to as *logical operators*. When applied to
numeric data types, they are referred to as *bitwise operators*.

Truth table:

|| x `&` y ||| x `\|` y ||| x `^` y ||
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **x**\\ **y**| **TRUE** | **FALSE** || **TRUE** | **FALSE** || **TRUE** | **FALSE** |
| **TRUE** | true | false || true | true || false | true |
| **FALSE** | false | false || true | false || true | false |

Short-circuit operators, `&&` and `||`, does not evaluate right-hand side
expression if the left-hand is sufficient.

#### Equality Operators

Used in 1 of 3 scenarios:

1. Comparing two numeric primitive types. Numeric promotion might take place.
2. Comparing two *boolean* values.
3. Comparing two objects, including *null* and *String*.

For object comparison, the equality operator is applied to references to the
object. Two references are equal if and only if they point to the same object or
they point to null. For objects, the equality operator is evaluating if objects
are the same, rather than if they are equivalent.

### Java Statements

*Control flow statements* is the way in which Java can break up the flow of execution.

#### Statements

- *if-then*
- *if-then-else*
- *switch*
  - optional default statement, branched to if no cases match
  - supported types:
    - *byte*, *Byte*
    - *short*, *Short*
    - *int*, *Integer*
    - *char*, *Character*
    - *String*
    - *enum*
    - *var* if the type resolves to one of the previous values
    - **Note**: *boolean*, *double*, *float* and *long* is **not** allowed.
  - target variable is evaluated at runtime
  - cases needs to be compile-time constants of the same data type as the switch
  value (literals, enum constants, or final keyword).
  - omitting break statements will result in fall-through
  - numerical promotion that does not need explicit cast is allowed
- *while*
- *do-while*
  - guarantees that the statement or block is evaluated at least once
- *for*
  - *for(initialization; booleanExpression; updateStatement)  {};*
  - initialization and update sections may contain multiple statements,
  separated by commas
    - **Note**: multiple assignments is not consistent with normal multiple assignment:
      - `x = 0, y = 0` is OK in for loops, but not otherwise.
      - Assigning values to variables of different types is OK in for loops, but
      not otherwise.
- *for-each*
  - right-hand side of the for-each loop statement must be a built in array or
  an object whose class implements *java.lang.Iterable*.

**Note**: if no bracers is used, only the next statement is evaluated.

### Advanced Flow Control

A **label** is an optional pointer to the head of statement that allows the
application flow to jump to it or break from it. It as a single word proceeded
by a colon. Labels follow the same naming rules as for identifiers.

The **break** statement transfers the flow of control to the enclosing statement.

The **continue** statement transfers the flow of control to the boolean
expression that determines if the loop should continue.

**Note**: label can be used in conjunction with either break or continue.

## Core Java APIs

_Mutable_ : an object that is changeable.

_Immutable_ : an object that can't be changed once it's created. Immutable
classes are final, and subclasses cannot add mutable behavior.

### Creating and Manipulating Strings

_String_ is immutable.
_StringBuilder_ is mutable.

They both implements the `CharSequence` interface.

The `+` operator can be used in two ways within the same line of code. These are
the 3 rules governing the `+` operator:

1. If both operands are numeric, + means numeric addition.
2. If either operand is a String, + means concatenation.
3. The expression is evaluated from left to right.

`+=` operator in concatenation: `s += "2"` means the same thing as `s = s + 2`.

#### The String Pool

Also know as *intern pool*, is located in the JVM. The string pool contains
literal values that appear in the program.

`object.toString()` is a String but not a literal, hence does not go to the
string pool. Strings not in the string pool are garbage collected as other objects.

`new String("some string")` forces the String object to not be included in the
string pool.

- JVM reuses String literals

```java
String a = "Hello";
String b = "Hello";
System.out.println(a == b); // true
```

In the code below _a_ and _b_ are not the same at compile time, hence a new
String is created for _b_.

```java
String a = "Hello";
String b = "   Hello".trim();
System.out.println(a == b); // false
System.out.println(a.equals(b)); // true
```

**Note**: `intern()` method can be used to tell Java to explicitly use the String pool

```java
String s1 = "Hello";
String s2 = new String("Hello").intern();
System.out.println(s1 == s2); // true
```

#### String

- Immutable.
- Implements _Serializable_, _CharSequence_, _Comparable<String>_.
- `equals()` is implemented.

##### Important String methods

**Note**: _length()_ gives length of String, but _indexOf()_ starts at 0.

- _length()_
- _charAt()_
- _indexOf()_
- _substring()_
  - the _endIndex_ parameter is allowed to be 1 past the end of the sequence.
- _toLowerCase()_, _toUpperCase()_
- _equals()_, _equalsIgnoreCase()_
- _startsWith()_, _endsWith()_
- _contains()_
- _replace()_, _replaceAll()_
  - If the char/String/regex is not included as parameter, these method return a reference
  to itself.
- _trim()_, _strip()_, _stripLeading()_, _stripTrailing()_
  - removes whitespace `\t` (tab), `\r` (carriage return) and `\n` (new line) from
  beginning and end of a String. _strip()_ also supports Unicode.

### StringBuilder

- Mutable.
- Implements _Serializable_, _Appendable_, _CharSequence_, _Comparable<StringBuilder>_.
- Does not override `equals()`, hence reference equality is default.

The _StringBuilder_ methods changes its own state and returns a reference to itself.
There are 3 ways to create a _StringBuilder_:

- _new StringBuilder();_
- _new StringBuilder("some string");_
- _new StringBuilder(10);_


**Note**: _StringBuilder_ is initialized with 16 capacity by default. When
initializing with String, the length of the string is added to that. Length
and capacity is not the same thing.

#### Important StringBuilder methods

- _charAt()_, _indexOf()_, _length()_, _substring()_
    - _substring()_ returns a String rather than a StringBuilder
  - _capacity()_
    - different from _length()_. StringBuilder default capacity is 16.
- _append()_
  - _append()_ has many signatures for converting to String, e.g.
  _sb.append(true)_ appends _"true"_.
- _insert()_
- _delete()_, _deleteCharAt()_
- _replace()_
  - allows the second index to be past the end of StringBuilder.
- _reverse()_
- _toString()_

**Note**: _StringBuffer_ has the same functionality but is thread safe.

### Equality

- If _equals()_ is not implemented, it will check for reference equality
(as `==` does).
- if `x.equals(y)` then `x.hashCode()` must equal `y.hashCode()`.

### Arrays

- Uses `[]` operator.
- An array is an area of memory on the heap with space for a *fixed number* of elements.
- An array is a *reference variable*, even if it is an array of primitives.
- Does not override `equals()`, hence reference equality is default.
- `length` is not a method.
- Can contain primitives and reference variables

Ways of creating an array:

```java
int[] numbers = new int[2];
int[] numbers = new int[] {42, 3, 11};
int[] numbers = {42, 3, 11}; // shorthand notation
```
**Note**: size parameter is needed if not initializing.

`[]` operator can be inserted in different places

```java
int[] numbers;
int [] numbers;
int numbers[];
int numbers [];
```

**Note**: When creating multiple arrays on the same line

```java
int[] a, b; // creates TWO arrays
int a[], b; // create ONE array, and ONE int
```

**Note**: When using array initilizer to create an array, it must be used in declaration.

```java
int[] numbers;
numbers = {42, 3, 11}; // DOES NOT COMPILE
```

#### Arrays With Reference Variables

```java
class Names {
  String[] names; // points to null
}
```

```java
class Names {
  String[] names = new String[2]; // points to an array with 2 null elements
}
```

#### Sorting

```java
import java.util.Arrays;
```

Use `Arrays.sort()`.

- Strings sorts numbers before letters, and uppercase before lowercase.

#### `Arrays.binarySearch()`

```java
Arrays.binarySearch(numbers, 2);
```

- Binary search can only be used on a sorted array, otherwise it is unpredictable.
- If the element is found, binarySearch will give the index. If the element is
not found, binarySearch will give the element where it should be inserted to
remain sorted as _-(indexToInsertAt + 1)_ (negative for not found, and minus one
in order to omit 0 index).

#### `Arrays.compare()`

```java
Arrays.compare(array1, array2)
```

Compares each element at a time.

- If array1 and array2 are equal it returns 0.
- If an element at the same postion is greater in array1, a positive number will be returned.
- If an element at the same position is greater in array2, a negative number will be returned.

- `null` is smaller than any other value
- numbers are smaller than letters
- uppercase is smaller than lowercase

#### `Arrays.mismatch()`

Returns -1 if arrays are equal, otherwise returns the first index where they differ.

#### Multidimensional Arrays

Arrays that can hold other arrays. Note that the sub-arrays does not have to be
of the same size.

```java
int[][] vars; //2D
int vars[][]: //2D
int[] vars[]  //2D
int[] vars1[], vars2[][] //2D and 3D
```

```java
String[][] rectangle = new String[3][2];
int[][] different = {{1, 2}, {3}, {4, 5, 6}};
```

Multidimensional arrays can be initialized with the **new operator** or an
**array initializer**.

```java
int[][] ints = new int[2][2];
int[][] ints = new int[][] {{1, 2},{3, 4}};
int[][] ints = {{1, 2},{3, 4}}; // shorthand notation
```

**Note**: When initializing a multidimensional array, the size of the first
dimension must be provided. The dimensions of other dimensions are not required
but a declaration in size cannot be preceded by an empty brackets. (Size of an
array is fixed, so it must be guaranteed that next array fits in the previous).

```java
int[][][] ints3d = new int[3][2][]; // COMPILES
int[][][] ints3d = new int[][][] // DOES NOT COMPILE
int[][][] ints3d = new int[][][3] // DOES NOT COMPILE
```

### List

- Implements _Iterable<E>_, _Collection<E>_.

List is an interface implemented by e.g. `ArrayList`.

The static factory methods

- `List.copyOf()`
- `List.of()`

returns an _unmodifiable_ list, with properties

- Cannot add, replace or remove elements (`UnsupportedOperationException`).
- Disallow `null` elements (generating `NullPointerException`).
- Serializable if all elements are.
- Identity-based operations are unstable and should be avoided (`==`, hash code,
synchronization).

The methods

`toArray()`
`toArray(T[] a)`

returns an array either of type `Object[]` or of type `T[]`. The returned array
will be "safe" in that no references to it are maintained by this list.

#### Sorting Lists

```java
Collections.sort(list);
```

### ArrayList

```java
import java.util.ArrayList;
```

- Can change in size.
- Implements _Serializable_, _Cloneable_, _Iterable<E>_, _Collection<E>_, _RandomAccess_
- Supports generics and reference variables (for primitives *wrapper classes* are
used instead)

3 ways to create an _ArrayList_ including elements of _Object_ type:

```java
ArrayList list1 = new ArrayList();
ArrayList list2 = new ArrayList(10);
ArrayList list3 = new ArrayList(list2); // Copy of list2
```

Using generics

```java
List<String> list = new ArrayList<>();
```

Using `var`

```java
var list1 = new ArrayList<String>(); // type of list one is ArrayList<String>
var list2 = new ArrayList<>(); // type of list2 is ArrayList<Object>
```

#### Important ArrayList methods

- _add()_
  - _boolean add(E element)_ always returns true.
  - _void add(int index, E element)_
- _remove()_
  - _boolean remove(Object object)_ true if a match was found.
  - _E remove(int index)_ removes object and returns it
- _E set(int index, E newElement)_ replaces element at index
- _isEmpty()_, _size()_
  - _isEmpty()_ is true if and only if _size()_ is 0.
- _clear()_ sets size to 0.
- _contains(Object object)_
- _boolean equals(Object object)_ true if contains the same elements in the same order.

### Wrapper Classes

Primitive types have corresponding wrapper classes.

Immutable.

All pairs have methods on the pattern of

- `int Integer.parseInt(String string)` returns primitive type (except for `Character`).
- `Integer Integer.valueOf(int i)` returns wrapper class

**Note**: `valueOf()` is prefered over constructor since it takes advantage of caching.

|Wrapper class|primitive type|
|:--|:--|
|`Boolean`|`boolean`|
|`Byte`|`byte`|
|`Short`|`short`|
|`Integer`|`int`|
|`Long`|`long`|
|`Float`|`float`|
|`Double`|`double`|
|`Character`|`char`|

**Note**: the _Character_ class does not participate in _parse/valueOf_ methods
since _String_ is made up of multiple _char_. However, _String.valueOf()_ exists
which uses the _toString()_ method in case an object is passed.

**Note**: increment and decrement operator works on wrapper classes.

#### Autoboxing and Unboxing

Since Java 5, primitive values will be automatically converted to the relevant
wrapper classes.

```java
List<Double> weights = new ArrayList<>();
weights.add(50.5);
weights.add(Double.valueOf(60.2));
```

The following code generates a _NullPointerException_ because Java tries to
convert _null_ to _int_.

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(null); // Integer is not a primitive
int n = numbers.get(0); // NullPointerException because unboxing fails
```

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(1);
numbers.add(2);
numbers.remove(1); // removes INDEX 1: [1]
numbers.add(2);
numbers.remove(new Integer(1)); // removes OBJECT 1: [2]
```

### Converting Between Array and List

#### List to Array

`toArray()` method creates a copy with no connection to the orginal list.

```java
List<String> list = new ArrayList<>();
list.add("a string");
Object[] arrayCopy1 = list.toArray() // Array default to Object types
String[] arrayCopy2 = list.toArray(new String[0]); // Array with String types (with corresponding size)
list.clear(); // does NOT affect arrayCopy1 or arrayCopy2
System.out.println(arrayCopy1.length); // 1
```

#### Array to List

There are two options: backed list or unmodifiable list.

##### Backed List

The original array and created array _backed List_ are linked. When change is
made in one, it is available in the other.

It is __fixed size__; we are not allowed to change the size.

```java
List<String> list = Arrays.asList(array);
list.set(1, "test");
list[0] = "new";
list.remove(1); // throws UnsupportedOperationException
```

**Note**: the _backed List_ is __not__ a `java.util.ArrayList`.

##### Unmodifiable List

```java
Sting[] array = {"abc", "def"};
List<String> list = List.of(array); // unmodifiable
```

The `List.of()` method creates an unmodifiable list with the values in the array. Unmodifiable
lists disallows `null` elements.

### Set

- Cannot contain duplicates.
- Unordered.
- `HashSet`, `TreeSet`.

- _boolean add()_ returns `true` if added, `false` otherwise.

### Map

- Key/value pairs.
- `HashMap`.

|method|description|
|:--|:--|
|`V get(Object key)`|returns value mapped by key or `null` if none is mapped|
|`V getOrDefault(Object key, V other)`|returns value mapped by key or other if none is mapped|
|`V put(K key, V value)`|adds or replaces key/value pair. Returns previous value or `null`|
|`V remove(Object key)`|removes and returns value. `null` if none|
|`boolean containsKey(Object key)`||
|`boolean containsValue(Object value)`||
|`Set<K> keySet()`|Returns set of all keys|
|`Collection<V> values()`|Returns `Collection` of all values|

### Math

|method|description|
|:--|:--|
|`double min(double a, double b)`||
|`float`, `int`, `long`||
|`double max(double a, double b)`||
|`float`, `int`, `long`||
|`long round(double num)`|round numer to `long`|
|`int round(float num)`|round number to `int`|
|`double pow(double number, double exponent)`||
|`double random()`|returns number greater or equal to 0 and strictly less than 1|

### Dates and Times

```java
import java.time.*;
```

- Static classes (`new` keyword is not allowed)
- Created object is immutable

|||
|:---|:---|
|*LocalDate*| _2015-01-20_ |
|*LocalTime*| _12:45:18.401_ |
|*LocalDateTime*| _2015-01-20T12:45:18.401_|
|*Period*| >= Days|
|*Duration*| =< Days|
|*DateTimeFormatter*||
|*FormatStyle*| _SHORT_, _MEDIUM_|

#### LocalDate

```java
LocalDate.of(2015, Month.JANUARY, 20);
LocalDate.of(2015, 1, 20);
```

#### LocalTime

```java
LocalTime.of(6, 15); // Hours and minutes
LocalTime.of(6, 15, 30); // + seconds
LocalTime.of(6, 15, 30, 200); // + nanos
```

#### LocalDateTime

```java
LocalDateTime.of(2015, Month.JANUARY, 20, 6, 15, 30);
LocalDateTime.of(localDate, localTime);
```

#### Period

```java
Period.ofYears(1);
Period.ofMonths(3);
Period.ofWeeks(3);
Period.ofDays(2);
Period.of(1, 0, 7); // Every year and 7 days

// NOTE
Period.ofYears(1).ofWeeks(3) // Every 3 weeks
```

#### Duration

```java
Duration.ofDays(1);
Duration.ofHours(3);
Duration.ofMinutes(3);
Duration.ofSeconds(2);
Duration.ofNanos(200);
```

#### Important methods

- _plusNanos()_, _minusNanos()_
- _plusSeconds()_, _minusSeconds()_
- _plusMinutes()_, _minusMinutes()_
- _plusHours()_, _minusHours()_
- _plusDays()_, _minusDays()_
- _plusWeeks()_, _minusWeeks()_
- _plusMonths()_, _minusMonths()_
- _plusYears()_, _minusYears()_
- _isBefore()_, _isAfter()_

#### Formatting Dates and Times

Format can be called from the date/time class or the DateTimeFormat class:

```java
date.format(DateTimeFormatter.ISO_LOCAL_DATE);
time.format(DateTimeFormatter.ISO_LOCAL_TIME);
dateTime.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME);

DateTimeFormatter shortDateTime = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT)
shortDateTime.format(dateTime);
shortDateTime.format(date);
```

```java
DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT)
DateTimeFormatter.ofLocalizedTime(FormatStyle.MEDIUM)
DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM)

DateTimeFormatter.ofPattern("MMMM dd, yyyy, hh:mm");
```

##### Parsing

Pattern can be specified. If not specified, the default is used

```java
DateTimeFormatter f = DateTimeFormatter.ofPattern("MM dd yyy");
LocalDate.parse("01 02 2015", f);
LocalTime.parse("11:22"); // Uses default parser
```

## Methods and Encapsulation

```java
public final void nap(int minutes) throws InterruptedException {}
```

|Element|Example|Required|
|:---|:---|:---|
|access modifier| `public` |N|
|optional specifier| `final` |N|
|return type| `void` |Y|
|method name| |Y|
|parameter list| |Y|
|exception| `throws InterruptedException` |N|
|method body| `{}` |Y|

**Note**: Optional specifiers are allowed to come before access modifier.

### Access Modifiers

|modifier|visibility|
|:---|:---|
|`public`| all |
|`protected`| package and subclass (even in other package) |
|_none_| package private |
|`private`| class |

#### Protected

A subclass in another package may access protected variables, but only if
accessed via the sub-classing object.

```java
package differentpackage;
import protectedpackage.ProtectedClass;

public class ThisClass extends ProtectedClass {
(...)
    ThisClass sc = new ThisClass();
    sc.inheritedProtectedMethod(); // OK

    ProtectedClass pc = new ProtectedClass();
    pc.inheritedProtectedMethod(); // DOES NOT COMPILE
```

### Optional Specifiers

Java allows optional specifiers to come before the access modifier.

|specifier|description|
|:---|:---|
|_static_| |
|_abstract_| |
|_final_| not allowed to be overridden by subclass|
|_synchronized_| |
|_native_| used when interacting with code written in other language|
|_strictfp_| used for making floating-point calculations portable|

### Varargs

- A vararg parameter must be the last element in a method's parameter list.
- An array can be passed as varargs, or values can be passed in which case Java
converts them into an array.
- A vararg parameter can be omitted.
- `null` can be explicitly passed.

### Static

Static methods don't require an instance of the class. They are shared among all
users of the class.

Static methods have two main purposes:

1. For utility and helper methods that don't require any object state.
2. For state that is shared by all instances of the class (methods that merely
uses that state should be static as well).

When accessing a static member, Java only checks reference type. This means that
the object can be null, and still the static member can be accessed:

```java
AClassWithStaticMembers s = null;
s.staticMember; // OK
```

Static members cannot call an instance member.

`final` variables cannot be reassigned. Primitive types cannot change value,
reference types cannot point to another object. If final values are not
initialized, they can be initialized in a _static initializer_.

#### Static initializers

_Static initializers_ can be the first assignment of _static final_ variables.
Static initializers are run only once.

```java
private static final int NUM_SEC_PER_HOUR;
static {
  int numSecPerMin = 60;
  int numMinPerHour = 60;
  NUM_SEC_PER_HOUR = numSecPerMin * numMinPerHour;
}
```

#### Static Imports

Static imports are for importing static members of classes.

Members of the class has preference; a method with the same name as the static
imported one will get precedence.

### Passing Data Among Methods

Java is "pass-by-value". A copy of the variable is made and the method receives
that copy. Assignments made in the method does not affect the caller.

Assigning a new primitive or reference does not change the caller. Calling
methods on an object does affect the caller.

Returning data from a method is done by copying the primitive of reference and
returning it.

### Overloading Methods

Java calls the most specific method it can.

_Method overloading_ occurs when there are different method signatures with the
same name but different type parameters.

#### Overloading order

1. Exact match by type
2. Larger primitive type
3. Autoboxed type
4. Varargs

**Note**: Java only converts in one step.

#### Varargs

The parameter type list cannot be the same when overloading. This is also the
case for varargs e.g.

```java
public void fly(int[] numbers) { }
public void fly(int... numbers) { } // DOES NOT COMPILE
```

#### Autoboxing

Autoboxing will occur if only the _Integer_ method is present, but if both are
present (as below) Java will pick the most specific one.

```java
public void fly(int number) { }
public void fly(Integer number) { } // COMPILES
```

```java
public void fly(Object obj) { }
this.fly(42) // COMPILES
```

#### Primitives

Java accepts wider types, otherwise explicit casting is required.

### Creating Constructors

- Creating new objects with constructors is *instantiation*.
- A constructor is called by using the `new` keyword. When this keyword occurs,
Java allocates memory for the new object on the heap.
- `this` tells Java to reference an instance variable.

**Note**: If there are *no* constructors present, Java generates a *default
constructor* during compile step (omitted in .java, but present in .class file).

### Constructor overloading

When `this` is called as a function, Java interprets it as a constructor call.
`this()` has to be the first non-commented statement in the constructor.

This can be used for *constructor chaining*

```java
public Hamster(int weight) {
    this(weight, "brown");
}
```

### Final

`final` variables must be assigned a value **exactly once**. Either in:

- the line of declaration,
- in an instance initializer,
- in the constructor.

By the time the constructor completes, all `final` instance variables must have
been set.

### Order of Initialization

1. *super class* if there is one
2. *static variable declarations* and *static initilizers* (in the order they appear)
3. *instance variable declarations* and *instance initializers* (in the order
they appear; they are copied to beginning of the constructor)
4. constructor

**Remark**: If the class is not initialized, only rules 1 and 2 applies.

**Remark**: If object is initialized in a static initializer, the object
initialization lands on top of the stack.

```java
public class OrderOfInit {
  static {System.out.println("printed first");}
  {System.out.println("printed second");}
  static {new OrderOfInit();}
  static {System.out.println("printed third");}
}
```

### Encapsulating Data

*getter* or *accessor method*.

*setter* or *mutator method*.

Java defines a naming convention that is used in *JavaBeans*. *JavaBeans* calls
an instance variable a *property*.

#### JavaBeans

|Rule|Example|
|:---|:---|
| Properties are private | `private int num`|
| Getters begin with *is* or *get* if the property is a boolean | `public boolan isBool` `public boolean getBool`|
| Getters begin with *get* if the property is not a boolean | `public int getNum` |
| Setters begins with *set* | `public void setNum` |
|The method name must have a prefix of *set*/*get*/*is*, followed by the name of the variable with first letter capital | |

#### Encapsulated and Immutable classes

Immutability refers to preventing callers from changing the instance variables.

Encapsulation refers to preventing callers from changing the instance variables
directly.

### Lambdas and Functional Interfaces

- A *lambda expression* is a block of code that is passed around.
- *Deferred execution* means the code is specified now but will be run later.
- Lambdas work with interfaces that have *only one* abstract method, called *Functional Interfaces*.

#### Lambda syntax

```java
a -> a.canHop()
(Animal a) -> {return a.canHop(); } // including optional parts
```

- Parenthesis can be omitted when there is a single parameter with no explicit type.
- Braces can be omitted when there is only one return statement.
- When using braces, `return` and `;` is mandatory.

Java doesn't allow us to redeclare local variables in lambdas

```java
(a, b) -> {int a = 0; return 5;} // DOES NOT COMPILE
```

#### Variable Accessability in Lambda Body

Method parameters and local variables are allowed to be referenced if they are *effectively final*.
This means that the value of the variable doesn't change after it is set, regardless of whether
it is explicitly marked as `final`.

|variable type|rule|
|:--|:--|
|instance variable|allowed|
|static variable|allowed|
|local variable|allowed if effectively final|
|method parameter|allowed if effectively final|
|lambda parameter|allowed|

#### Functional Interfaces

|Functional Interface|method|example|
|:--|:--|:--|
|Predicate|`boolean test(T t)`|`removeIf()` in a `List` or `Set`|
|Consumer|`void accept(T t)`|`forEach()` in a `List`, `Set` or `Map`|
|Supplier|`T get()`||
|Comparator|`int compate(T o1, T o2)`|`sort()` in a `List`|

##### Predicate

In the package `java.util.function`.
*ArrayList* declares `removeIf()` that takes a `Predicate`.

## Class Design

- Inheritance
- Override
- Hiding
- `final`
- `abstract`

### Classes

`private` and `protected` modifiers can only be applied to inner classes.

Interfaces is the only construct that adds implicit keywords.

#### Constructors

If a class has no parent, Java automatically adds `extends java.lang.Object`.

If a class has no constructor, Java automatically adds a no-argument
constructor (this constructor will contain a no-argument `super()` call). The
added constructor will have the same visibility as the class.

The first call of a constructor is either a `this()`-call to another constructor
or a `super()`-call to its parent. A no argument `super()` is inserted if none
of these are explicitly added.

```java
public class Foo {
  public Foo(int baz) {
  }
}

public class Bar extends Foo {
  public Bar() { // DOES NOT COMPILE
  }
}
```

The parent constructor is always executed before the child constructor.

#### Inhereting Methods

**Note**: `final` methods cannot be overridden nor hidden.

##### Overriding a Method

Methods can be overridden by declaring a new method with the same signature and
return type as in the child class.

Overriding is performed at runtime, meaning that which method is called is
decided by the object being referred to (not the type of the reference variable).

Overridden methods can be accessed with the `super` keyword.

Rules for overriding non-private methods:

1. Method must have same signature.
2. Method must be at least as accessible.
3. Method may not throw a checked exception that is new or broader than the
class of any exception thrown in the parent class.
4. If returning a value, it must be the same type or a subclass of the parent's
return value type (known as _covariant return types_).

**Note**: Only methods can be overridden. Variables are _hidden_.

**Note**: These rules does not apply to `private` methods, since they cannot be overridden.

**Note**: Without these rules, it is possible to create contradictions in the language.

###### Explaing Rule 2

```java
// other package
SuperClass anInstance = new SubClass();
anInstance.protectedMethodInSubclass(); // Contradicting ambiguity of accessability
```

###### Explaining 3

```java
public SuperClass {
  public void method() {...}
}

public SubClass {
  public void method() throws Exception {...}
}

SuperClass anInstance = new SubClass();
anInstance.method(); // Contradicting ambiguity in Exception thrown
```

###### Explaining 4

```java
public SuperClass {
  public Double method() {...}
}

public SubClass {
  public Number method() {...}
}

static Double aFunction(SuperClass anInstance) {
  // Contradiction if SubClass is sent as parameter and returns eg an Integer
  return anInstance.method();
}
```

##### Hiding Static Methods

_hidden method_: a static method with same name and signature as static method
in parent class.

Rules for hiding static methods are the same above with one additional rule:

5. `static` keyword must match use of static keyword in parent class (ie. a non
static method cannot be made static and vice verca).

**Note**: The child version of an _overridden_ method is always executed,
regardles of where the call is made from. However, the version of a hidden
method depends on from which class it is called from.

#### Inhereting Variables

Java does not allow variables to be overridden, only hidden.

Variables with same names as in parent class are hidden; there exists two
versions of them.

Refering to paren't version from the child class can be done with the `super` keyword.

**Note**: Hidden variables does not need to be of the same type as the one it's hiding.

### Abstract Classes and Methods

Marked with `abstract` keyword.

Abstract class rules:

1. Abstract classes cannot be instantiated.
2. May include any number of abstract or nonabstract methods.
3. May not be marked as `private`, `protected` or `final`.
4. An abstract class extending another abstract class inherits all abstract
methods as its own.
5. The first _concrete class_ extending an abstract class must provide
implementation for all abstract methods.

Abstract method rules:

1. Defined in an abstract class.
2. May not be declared `private` or `final`.
3. May not have an implementation in the abstract class.
4. Implementing an abstract class follow the same rules as overriding.

### Interfaces

Interfaces are by definition `abstract`.

A class invokes an interface by using the `implements` keyword.

Rules for defining an interface:

1. Interfaces cannot be instantiated directly.
2. Interfaces may have any number of methods.
3. Interfaces may not be declared as `final`.
4. Top-level interfaces are `abstract` by definition (thus `private`,
`protected` and `final` methods result in compiler error).
5. Methods (except `static` and `default`) are assumed to be `public` and
`abstract` by default (marking these with `private`, `protected` or `final`
will trigger compiler error).

**Note**: An interface `extends` another interface (it does not `implements` them).

#### Default Methods

`default` methods are not `abstract`.

- `public`.
- Cannot be marked as `static`.
- Requires an implementation.

**Note**: and interface or abstract class may redeclare a `default` method and
mark it as `abstract` (without implementation).

#### Static Methods

`static` interface methods are not `abstract`.

**Note**: `static` interface methods are not inherited by classes implementing
the interface (to reference a static interface method, the name of the interface
must be used).

#### Private Methods

Since Java 9, `private` and `private static` methods are allowed.

Rules for private methods in interfaces:

1. `private` cannot be `abstract` (requires a body).
2. `private` methods can only be used inside the interface.
3. `private static` methods can be used inside other static and non-static
interface methods.
4. `private` non-static methods cannot be used inside private static methods.

#### Interface Variables

Interface variables are `public`, `static` and `final`. These are added implicitly
if not explicitly added.

### Polymorphism

Polymorphism in Java is that an object may be accessed using a reference of

- the object's type,
- a superclass' type
- an implemented interface's type (even through a superclass)

The object is the entity that exist in the memory, allocated by the JRE.
Regardless of what type the reference has, the object does not change. However,
the type of reference changes what variables and methods are accesible.

```java
class Foo {}

class Bar extends Foo {
  Foo foo = new Foo();
  Bar bar = (Bar) foo; // ClassCastException. Does not give access to Bar
}
```

#### Casting

Access to variables and methods can be regained if explicitly casting the object.

Rules for casting:

1. Casting from subclass to superclass does not require explicit cast.
2. Casting from superclass to subclass does require explicit cast.
3. The compiler does not allow casts to unrelated types.
4. Even if the code compiles, an exception may be thrown at runtime if the
object being cast is not actually an instance of that class.

**Note**: Illegal castings will generate `ClassCastException` at runtime.

#### Virtual Methods

_virtual method_: A method where the implementation is not determined until runtime.

**Note**: All non-`final`, non-`static`, non-`private` are considered virtual
since they can be overridden at runtime.

#### Polymorphic Parameters

The ability to pass instances of a subclass or interface to a method.

## Exceptions

An Exception is an event that alters the program flow.

There are two possibilities in dealing with exceptions in Java

1. A method can handle the exception on it's own.
2. A method makes it the caller's responsibility to handle the exception.

Java has a `Throwable` superclass for all objects that alter program flow in
this aspect.

- `java.lang.Throwable`
  - `java.lang.Exception`
    - `java.lang.RunTimeException`
  - `java.lang.Error`

### Errors

Subclasses of `java.lang.Error` are thrown by the JVM and should not be handled
or declared.

Common errors:
| **Error** ||
|:--|:--|
|`ExceptionInInitializerError`|static initializer throws unhandled exception|
|`StackOverflowError`|a method calls itself too many times|
|`NoClassDefFoundError`|class used is available at compile time but not runtime|

### Exceptions

A `RunTimeException` and its subclasses is also known as an _unchecked exception_.

An `Exception` (but not a `RunTimeException`) is also known as an _checked exception_.

The `throw` keyword tells Java that another part of the code should take care of
the exception.

|**Type**|**Okay to catch?**|**Required to catch?**|
|:--|:--|:--|
|RunTimeException|Yes|No|
|Checked Exception|Yes|Yes|
|Error|No|No|

#### Checked Exceptions

_handle or declare rule_: Java requires the code to either handle them or
declare them in the method signature.

```java
void fall() throws Exception {
  throw new Exception();
}
```

Common checked exceptions:

| **Exception** | **Thrown by** ||
|:--|:--|:--|
| `FileNotFoundException` | Programmer | subclass of `IOException`|
| `IOException` | Programmer ||

#### Unchecked Exceptions (`RunTimeException`)

Common unchcked exceptions:

|**Exception**|**Thrown by**||
|:--|:--|:--|
| `ArithmeticException` | JVM ||
| `ArrayIndexOutOfBoundsException` | JVM ||
| `ClassCastException` | JVM ||
| `IllegalArgumentException` | Programmer ||
| `NullPointerException` | JVM ||
| `NumberFormatException` | Programmer | subclass of `IllegalArgumentException`|

#### Try-Catch-Finally Statement

Try statements separates the logic that might throw an exception from the logic
that handles that exception.

```java
try {
  // protected code
} catch (exception_type identifier) {
  // exception handler
} catch (exception_type_2 identifier) {
  // exception handler
}
...
} finally {
  // will always execute
}
```

**Note**: A `try` statement requires `catch` and/or `finally`. Both is not
required, but `catch` is required to come before `finally`.

**Note**: Code in `finally` always executes, regardless of wether an exception
was thrown. If no exeption is thrown, the `finally` clasue is run after the
`try` clause. If the an exception is thrown, the `finally` clause is run after
the `catch` clause.

**Note**: There is one exception to the rule above: if `System.exit(0);` is invoked.

**Note**: Wrapping code in a try-catch block that does not throw an exception
will not compile.

The code below hides the `RunTimeException` thrown from the `catch` clause.
Since `finally` is run afterwards and throws an `Exception`, the
`RunTimeException` is forgotten.

```java
try {
  throw new RunTimeException();
catch (RunTimeException e) {
  thrown new RunTimeException();
} finally {
  throw new Exception();
}
```

##### Chaining Catch Statements

`catch` blocks are evaluated in order. At most one can be executed.

A `catch` statement catching a superclass before a subclass is not allowed. This
generates a compiler error for unreachable code.

When a catch block specifies multiple exception types, these types must be
disjoint and the variable is implicitly `final`.

```java
catch (Exception | IOException e) { // DOES NOT COMPILE
```

#### Subclasses

When implementing or overriding methods, it is not allowed to add new checked
exceptions to the method signature.

A subclass is allowed to declare fewer exception that the superclass or interface.

An overridden or implemented methods is allowed to declare a subclass of a
checked exception type.

**Note**: Declaring a new `RunTimeException` is legal (since it's redundant;
unchecked exceptions are not required to be declared)

## Modules

_Java Platform Module System_ (JPMS).

The main purpose of modules is to provide groups of related packages to offer a
particular set of functionality to developers.

_module_: a group of one or more packages and a _module-info.java_ file.

Modules act as a fifth level of access control; they can expose packages within
the modular JAR to specific other packages.

`java` and `javac` takes flag `-p <path>` or `--module-path <path>` to specify the module path.

`java` takes flag `-m <names>` or `--module <name>` to specify module name and class.

Compile the module

```sh
javac -p mods -d classFilesDir package/name/*.java package/module-info.java
```

Running the module

```sh
java -p mods -m module.name/package.name.className
```

Packaging module

```sh
jar -cvf mods/package.name.jar -C classFilesDir/ .
```

### `module-info.java`

#### `exports`

`exports` indicates the module is to be used by others. Can be used to export to a specific
module also

```java
module package.name {
  exports package.name to other.module;
}
```

#### `requires`

Specifies that the current module depends on the required module.

`requires transitive` specifies that modules requiring the current module will also depend
on th transitive module.

```java
module package.name {
  requires transitive other.module;
}
```

### Command Line Options

`javac` module commands

|flag|description|
|:--|:--|
|`-cp <classpath>`| location of JARs in a nonmodular program|
|`-classpath <classpath>`||
|`--class-path <classpath>`||
|`-d <dir>`| directory to place generated class files|
|`-p <path>`| location of JARs in a modular program|
|`--module-path <path>`||

`java` module commands

|flag|description|
|:--|:--|
|`-p <path>`| location of JARs in a modular program|
|`--module-path <path>`||
|`-m <name>`| module name to run|
|`--module <name>`||
|`-d`| describe details of module|
|`--describe-module`||
|`--list-modules`| list observable modules without running program|
|`--show-module-resolution`| show modules when running program|

`jar` module commands

|flag|description|
|:--|:--|
|`-c`| create a new JAR file|
|`--create`||
|`-v`| prints details|
|`--verbose`||
|`-f`| JAR filename|
|`--file`||
|`-d`| describe details of module|
|`--describe-module`||

`jdeps` module commands

|flag|description|
|:--|:--|
|`--module-path <path>`| location of JARs in a modular program|
|`-s`| summarize output|
|`-summary`||

Compile the module

```sh
javac -p moduleFolderName -d directory classesToCompileIncludingModuleInfo
```

Run the module

```sh
java -p moduleFolderName -m moduleName/package.className
```

Describe a module

```sh
java -p moduleFolderName -d moduleName
jar -f jarName -d
```

List available modules

```sh
java -p moduleFolderName --list-modules
```

View dependencies

```sh
jdeps -s --module-path moduleFolderName jarName
```

Show module resolution

```sh
java --show-module-resolution -p moduleFolderName -d moduleName
```

