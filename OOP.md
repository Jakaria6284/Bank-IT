# Object-Oriented Programming (OOP) — MCQ Question Bank
**Subject:** Computer Science · **Target:** BCS Preliminary / Bank IT / NTRCA / GATE
**Questions:** 57 · **Concepts covered:** 51/51
**Languages:** C++ and Java, tagged per question — 40% `[C++]`, 35% `[Java]`, 25% `[Both]`/contrast

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.
> Read every **📘 CONCEPT** block once you finish a section — that's what actually
> transfers to questions you haven't seen before.

---

## Section 1 — Foundations: Objects, Classes, and the Four Pillars

**Q1.** Which of the following best defines encapsulation? `[Both]` `[Core]`
- **A)** Hiding the implementation of a function inside a loop
- **B)** Bundling data and the methods that operate on it into one unit, restricting direct access to internals
- **C)** Allowing a class to inherit from more than one parent
- **D)** Writing a function that behaves differently for different argument types

**Answer: B) Bundling data and the methods that operate on it into one unit, restricting direct access to internals**

**Trace / Why:** Encapsulation is a packaging discipline — data plus behavior in one unit, with access controlled through methods rather than exposed fields.

**📘 CONCEPT — The four pillars are independent tools, not synonyms**
> Encapsulation, abstraction, inheritance, and polymorphism solve four *different*
> problems: encapsulation controls **access** to state, abstraction controls
> **what is exposed** conceptually, inheritance controls **code reuse via
> hierarchy**, and polymorphism controls **which code runs for a given call**.
> A question naming one of these is asking about that specific problem, not
> "OOP in general."
>
> **Applies when** a stem says "hides implementation," "restricts access,"
> "combines data and behavior," or asks to distinguish two of the four pillars.
>
> **Boundary:** abstraction and encapsulation are often confused — abstraction
> hides *complexity* (what you don't need to know), encapsulation hides *access*
> (what you're not allowed to touch). A class can expose a simple abstraction
> while still leaking data through public fields (poor encapsulation, fine
> abstraction) — the two can vary independently.

**Wrong traces:** A = not a real OOP concept, distractor noise · C = describes multiple inheritance · D = describes polymorphism.

---

**Q2.** What is the key difference between abstraction and encapsulation? `[Both]` `[Trap]`
- **A)** Abstraction hides complexity by exposing only essential features; encapsulation hides internal state by restricting access
- **B)** They are two names for the exact same mechanism
- **C)** Abstraction is achieved only through interfaces; encapsulation only through private fields
- **D)** Encapsulation is a design-time concept; abstraction only exists at runtime

**Answer: A) Abstraction hides complexity by exposing only essential features; encapsulation hides internal state by restricting access**

**Trace / Why:** See the concept block in Q1 — one hides *what* you need to know, the other hides *how you can touch* the data.

**Wrong traces:** B = collapses two distinct pillars · C = overly narrow, both can be achieved multiple ways (abstract classes too, access modifiers plus properties too) · D = false dichotomy, both are design-time decisions with runtime effect.

---

**Q3.** In OOP terms, an **object** is best described as: `[Both]` `[Core]`
- **A)** A runtime instance of a class, holding its own state and able to invoke the class's behavior
- **B)** A named region of memory storing only a single primitive value
- **C)** The source file that defines a class
- **D)** A function that has no return type

**Answer: A) A runtime instance of a class, holding its own state and able to invoke the class's behavior**

**Trace / Why:** A class is the blueprint; an object is memory allocated according to that blueprint, carrying its own copy of instance data.

**Wrong traces:** B = ignores that an object bundles data *and* behavior · C = confuses source code with runtime entity · D = describes a constructor loosely and incorrectly.

---

**Q4.** Objects in an OOP system primarily interact by: `[Both]` `[Core]`
- **A)** Directly modifying each other's private memory addresses
- **B)** Sending messages — i.e., invoking each other's methods
- **C)** Sharing a single global variable pool
- **D)** Compiling into the same binary

**Answer: B) Sending messages — i.e., invoking each other's methods**

**Trace / Why:** The message-passing model is foundational to OOP: an object doesn't reach into another's internals, it asks the other object to do something on its behalf.

**Wrong traces:** A = violates encapsulation, is not how well-designed OOP works · C = describes procedural global-state style, the opposite of OOP · D = a build detail, unrelated to the runtime interaction model.

---

**Q5.** Who is credited with inventing the concept of object-oriented programming (via Smalltalk)? `[Both]` `[Core]` `[Asked: Sanfoundry]`
- **A)** Dennis Ritchie
- **B)** Bjarne Stroustrup
- **C)** Alan Kay
- **D)** James Gosling

**Answer: C) Alan Kay**

**Trace / Why:** Alan Kay led the Smalltalk work at Xerox PARC and coined the term "object-oriented programming"; Smalltalk is widely cited as the first purely object-oriented language.

**Wrong traces:** A = invented C, a procedural language · B = created C++, which added OOP to C but didn't invent the paradigm · D = created Java, decades after OOP already existed.

---

**Q6.** Which of these is generally **not** guaranteed simply by using an OOP language? `[Both]` `[Trap]`
- **A)** Code reusability through inheritance
- **B)** Modularity through classes
- **C)** Absence of duplicate or redundant data
- **D)** Encapsulated access to state

**Answer: C) Absence of duplicate or redundant data**

**Trace / Why:** OOP gives you the *tools* for reuse and modularity, but duplicate data still depends on how the programmer designs the classes — the paradigm doesn't enforce it.

**Wrong traces:** A, B, D = all directly follow from using inheritance, classes, and access modifiers correctly — genuine OOP guarantees when used as intended.

---

**Q7.** A "has-a" relationship between two classes, where one class contains an instance of another as a member, is called: `[Both]` `[Applied]`
- **A)** Inheritance
- **B)** Polymorphism
- **C)** Composition
- **D)** Overriding

**Answer: C) Composition**

**Trace / Why:** Composition models "has-a" by embedding an object as a field; inheritance models "is-a" by extending a type.

**📘 CONCEPT — Composition vs inheritance**
> Inheritance should only be used when the derived class genuinely **is a**
> specialised version of the base (a `Car` is a `Vehicle`). If the relationship
> is really "uses" or "has," composition is the correct tool — it also avoids
> fragile-base-class problems since the containing class isn't exposed to every
> change in the contained class's hierarchy.
>
> **Applies when** a stem describes one class holding a reference/field of
> another type, or asks "is-a vs has-a."
>
> **Boundary:** a `Car` *has an* `Engine` (composition) but *is a* `Vehicle`
> (inheritance) — the same design can use both relationships for different pairs
> of classes.

**Wrong traces:** A = is-a, not has-a · B = unrelated pillar · D = a polymorphism mechanism, not a relationship type.

---

## Section 2 — Constructors, Destructors, and Object Lifetime

**Q8.** What is the primary purpose of a constructor? `[Both]` `[Core]`
- **A)** To initialize an object's state at the moment it is created
- **B)** To free memory when an object is destroyed
- **C)** To convert one class type into another
- **D)** To declare a class as abstract

**Answer: A) To initialize an object's state at the moment it is created**

**Trace / Why:** A constructor runs automatically when an object comes into existence, guaranteeing the object starts in a valid state rather than with garbage/default memory.

**Wrong traces:** B = describes a destructor · C = describes a conversion/cast constructor, a narrow special case, not the general purpose · D = an unrelated keyword decision, not a constructor's job.

---

**Q9.** Can a constructor be overloaded? `[Both]` `[Core]`
- **A)** No, a class may have only one constructor
- **B)** Yes, by defining multiple constructors with different parameter lists
- **C)** Only in Java, never in C++
- **D)** Only if the class has no destructor

**Answer: B) Yes, by defining multiple constructors with different parameter lists**

**Trace / Why:** Constructor overloading works exactly like function overloading: same name, different parameter signatures, resolved at compile time by the arguments supplied.

**Wrong traces:** A = false, this is one of the most common OOP features · C = both languages support it identically · D = destructors are irrelevant to whether constructors can be overloaded.

---

**Q10.** Can a destructor be overloaded? `[C++]` `[Trap]`
- **A)** Yes, as long as parameter types differ
- **B)** Yes, but only with default arguments
- **C)** No — a class can have only one destructor, and it takes no parameters
- **D)** Only in a derived class, never in the base class

**Answer: C) No — a class can have only one destructor, and it takes no parameters**

**Trace / Why:** A destructor is invoked implicitly by the runtime with no way to pass arguments, so there is nothing to overload on — exactly one destructor per class is allowed.

**Wrong traces:** A, C = both wrongly assume destructors accept parameters · D = destructor rules apply identically at every level of a hierarchy.

---

**Q11.** In a multi-level inheritance chain `R : Q`, `Q : P`, in what order are the destructors called when an object of `R` is destroyed? `[C++]` `[Applied]` `[Asked: GATE]`
- **A)** P → Q → R
- **B)** All destructors run simultaneously
- **C)** Q → R → P
- **D)** R → Q → P

**Answer: D) R → Q → P**

**Trace / Why:** Construction goes base-to-derived (P → Q → R) so that a derived object's base parts exist before its own initialization runs. Destruction is the exact mirror: derived-to-base (R → Q → P), so the most-derived part is torn down first, while the base parts it might depend on are still intact.

**📘 CONCEPT — Construction and destruction are mirror-image orders**
> Construction order: base → derived. Destruction order: derived → base. This
> holds for member initialization too — members are constructed in declaration
> order (not initializer-list order) and destroyed in the reverse of that order.
>
> **Applies when** a stem gives an inheritance chain and asks for
> construction/destruction sequence, or shows an initializer list that doesn't
> match declaration order.
>
> **Boundary:** the *initializer list order in the constructor* has zero effect
> on construction order — only declaration order in the class body matters. A
> question that reorders the initializer list to mismatch the declared order is
> testing whether you know the list order is cosmetic.

**Wrong traces:** A = reverses the actual rule (this is the construction order, not destruction) · C = arbitrary, doesn't respect either mirror rule · B = destructors always run sequentially, never concurrently, in a single-threaded teardown.

---

**Q12.** Which of the following is true about a **copy constructor**? `[C++]` `[Core]`
- **A)** It is called only when an object is explicitly deleted
- **B)** It initializes a new object as a copy of an existing object of the same class
- **C)** It converts an object of one class into an unrelated class
- **D)** It can only be defined for abstract classes

**Answer: B) It initializes a new object as a copy of an existing object of the same class**

**Trace / Why:** A copy constructor takes a reference to an existing object (usually `const ClassName&`) and builds a new object with the same field values, invoked on pass-by-value, return-by-value, and explicit copy-initialization.

**Wrong traces:** A = describes destructor timing, not copy construction · C = describes a conversion between unrelated types, not copying · D = abstract classes can't be instantiated at all, so this is contradictory.

---

**Q13.** A class does a shallow copy (default compiler-generated copy constructor) on a class holding a raw pointer to heap memory. What is the most likely consequence? `[C++]` `[Trap]`
- **A)** A compile-time error, since pointers cannot be copied
- **B)** The program silently allocates a new block automatically for the second object
- **C)** The pointer is automatically deep-copied by the compiler
- **D)** Both objects end up pointing to the same heap block, risking a double-free when both destructors run

**Answer: D) Both objects end up pointing to the same heap block, risking a double-free when both destructors run**

**Trace / Why:** The default copy constructor copies the pointer's bit pattern (the address), not the memory it points to. Two objects sharing one address is fine until both destructors try to `delete` it — the second delete is undefined behavior.

**📘 CONCEPT — Shallow copy vs deep copy**
> The compiler-generated copy constructor and `operator=` perform a memberwise
> (shallow) copy: value types are duplicated, but pointer members just copy the
> address. If a class owns a resource via a raw pointer, it needs a
> user-defined copy constructor, assignment operator, and destructor (the
> "Rule of Three") to perform a genuine deep copy.
>
> **Applies when** a stem involves a class with a raw pointer/heap member and
> asks what happens on copy, or mentions "double free," "dangling pointer after
> copy."
>
> **Boundary:** this is a C++-specific trap. In Java, there are no raw pointers
> to manage this way — object references are copied (also "shallow" in a sense)
> but the garbage collector, not `delete`, reclaims memory, so there is no
> double-free risk.

**Wrong traces:** A = pointers copy just fine, that's exactly the problem · C = the *default* generated constructor never deep-copies automatically, that requires explicit code · B = no automatic allocation happens without user-written logic.

---

**Q14.** Why does Java have no concept of a destructor in the C++ sense? `[Java]` `[Trap]`
- **A)** Java objects never need cleanup
- **B)** Java's garbage collector reclaims unreachable objects automatically, removing the need for deterministic manual destruction
- **C)** Java forbids dynamic memory allocation entirely
- **D)** Java constructors already free memory when called twice

**Answer: B) Java's garbage collector reclaims unreachable objects automatically, removing the need for deterministic manual destruction**

**Trace / Why:** Since the JVM tracks reachability and frees memory itself, there's no fixed point where "the object is definitely gone" the way there is with C++'s `delete` — so a deterministic destructor doesn't fit the model. `finalize()` existed as a rough analogue but is deprecated since Java 9 precisely because its timing is unreliable.

**Wrong traces:** A = false, resources like file handles still need explicit closing (`try-with-resources`) · C = Java allocates objects on the heap constantly via `new`, it just doesn't expose raw pointers · D = constructors and destructors are unrelated lifecycle events, this option conflates them.

---

## Section 3 — Inheritance

**Q15.** How many principal types of inheritance are typically identified in C++? `[C++]` `[Core]` `[Asked: Sanfoundry]`
- **A)** 3
- **B)** 4
- **C)** 5
- **D)** 6

**Answer: C) 5**

**Trace / Why:** Single, multilevel, multiple, hierarchical, and hybrid — the five commonly enumerated forms, with hybrid being any combination of the others (and the usual source of diamond-shaped hierarchies).

**Wrong traces:** A, B = undercount, missing hybrid and/or hierarchical · D = overcounts; there is no commonly cited sixth distinct form.

---

**Q16.** Does Java support multiple inheritance of classes the way C++ does? `[Java]` `[Core]` `[Asked: examveda]`
- **A)** No — a class extends at most one superclass, though it may implement multiple interfaces
- **B)** Yes, identically to C++
- **C)** Yes, but only for abstract classes
- **D)** No, Java doesn't support inheritance at all

**Answer: A) No — a class extends at most one superclass, though it may implement multiple interfaces**

**Trace / Why:** Java's designers deliberately dropped multiple class inheritance to avoid the diamond problem, while still allowing a class to implement several interfaces since interfaces (pre-Java 8) carried no state to conflict over.

**Wrong traces:** B = directly false, this is the key point of divergence between the two languages · C = abstract classes follow the same single-inheritance rule as concrete ones · D = Java fully supports single-class inheritance and interface implementation.

---

**Q17.** The diamond problem arises when: `[Both]` `[Core]` `[Asked: examveda]`
- **A)** A derived class inherits from two base classes that themselves share a common ancestor, creating ambiguity about which copy of the ancestor's members to use
- **B)** A class has more than four methods
- **C)** A class inherits from itself recursively
- **D)** Two unrelated classes happen to have identically named methods

**Answer: A) A derived class inherits from two base classes that themselves share a common ancestor, creating ambiguity about which copy of the ancestor's members to use**

**Trace / Why:** Picture `Child : Parent1, Parent2` where both `Parent1` and `Parent2` inherit from `Base`. `Child` ends up with two separate copies of `Base`'s members, and a bare `child.member` doesn't know which copy to use.

**Wrong traces:** B = arbitrary, unrelated to method count · C = self-inheritance isn't legal and isn't what "diamond" describes · D = unrelated classes with same-named methods is ordinary overloading/shadowing, not the diamond problem (no shared ancestor).

---

**Q18.** How is the diamond problem's ambiguity resolved in C++? `[C++]` `[Applied]`
- **A)** The compiler always picks the leftmost base class automatically
- **B)** C++ simply forbids this inheritance pattern at compile time
- **C)** Using the scope resolution operator to disambiguate, or declaring the shared base as `virtual` so only one copy exists
- **D)** Java-style default interface methods handle it automatically

**Answer: C) Using the scope resolution operator to disambiguate, or declaring the shared base as `virtual` so only one copy exists**

**Trace / Why:** `d.Parent1::fun()` explicitly picks a path, while `class Parent1 : virtual public Base` ensures the compiler keeps a single shared `Base` subobject regardless of how many paths lead to it.

**Wrong traces:** A = an ambiguous call is a compile error, not silently resolved by position · B = the pattern compiles fine until you touch the ambiguous member without qualification · D = that's a Java 8+ mechanism for interface default methods, unrelated to C++ class inheritance.

---

**Q19.** In an access-controlled inheritance `class Derived : protected Base`, a `public` member of `Base` becomes, as seen from outside `Derived`: `[C++]` `[Applied]`
- **A)** Still `public`
- **B)** `protected`
- **C)** `private`
- **D)** Inaccessible entirely, even to `Derived`'s own methods

**Answer: B) `protected`**

**Trace / Why:** Protected inheritance caps every inherited member at `protected` or tighter — a `public` base member becomes `protected` in `Derived`, still visible to `Derived` and further subclasses but no longer to outside code.

**Wrong traces:** A = ignores that the inheritance keyword caps visibility, that's the whole point of protected/private inheritance · C = private inheritance would cap it to private, not protected inheritance · D = `Derived`'s own methods and further subclasses can still use it — it isn't sealed off entirely.

---

**Q20.** Which statement about interfaces implementing multiple inheritance-like behavior in Java is correct? `[Java]` `[Applied]`
- **A)** A class can implement only one interface
- **B)** Implementing an interface is identical to extending a class in every respect
- **C)** Interfaces in Java can have constructors like classes
- **D)** A class can implement multiple interfaces, inheriting their abstract method contracts (and, since Java 8, default method bodies)

**Answer: D) A class can implement multiple interfaces, inheriting their abstract method contracts (and, since Java 8, default method bodies)**

**Trace / Why:** This is exactly how Java gives programmers most of the reuse benefit of multiple inheritance (contracts, even shared default logic) while sidestepping the diamond problem for instance state, since interfaces (pre-Java 8) carried none.

**Wrong traces:** A = false, `implements A, B, C` is valid Java · C = an interface can't be instantiated so a constructor makes no sense · B = `extends` establishes an is-a relationship with inherited state and a superclass constructor chain; `implements` only establishes a contract.

---

**Q21.** `[Code-output]` What does this program print? `[C++]` `[Applied]` `[Code-output]`
```cpp
#include <iostream>
using namespace std;
class Base {
public:
    Base() { cout << "Base "; }
    ~Base() { cout << "~Base "; }
};
class Derived : public Base {
public:
    Derived() { cout << "Derived "; }
    ~Derived() { cout << "~Derived "; }
};
int main() {
    Derived d;
    return 0;
}
```
- **A)** `Derived ~Derived Base ~Base`
- **B)** `Base Derived ~Base ~Derived`
- **C)** `Base Derived ~Derived ~Base`
- **D)** `Derived Base ~Base ~Derived`

**Answer: C) `Base Derived ~Derived ~Base`**

**Trace / Why:** Construction runs base-first: `Base()` prints "Base", then `Derived()` prints "Derived". At the end of `main`, `d` goes out of scope and destructors mirror that order in reverse: `~Derived()` prints "~Derived" first, then the implicit base destructor call prints "~Base".

**Wrong traces:** A = swaps construction order (would require Derived built before its own Base part exists, impossible) · B = correct construction order but wrong destruction order (destruction is derived-first, not base-first) · D = incorrect on both halves.

---

## Section 4 — Polymorphism: Overloading, Overriding, and Binding

**Q22.** Compile-time polymorphism in OOP is most commonly achieved through: `[Both]` `[Core]`
- **A)** Virtual functions
- **B)** Function/method overloading (and operator overloading in C++)
- **C)** Dynamic method dispatch
- **D)** Abstract classes

**Answer: B) Function/method overloading (and operator overloading in C++)**

**Trace / Why:** Overloading is resolved by the compiler purely from the argument list at the call site — no runtime object-type lookup is involved, which is exactly what "compile-time" means here.

**Wrong traces:** A, C = both describe runtime (dynamic) polymorphism, resolved by the actual object type at execution time · D = abstract classes enable runtime polymorphism through pure virtual/abstract methods, not compile-time.

---

**Q23.** Runtime polymorphism in C++ requires a base class method to be declared: `[C++]` `[Core]`
- **A)** `static`
- **B)** `inline`
- **C)** `const`
- **D)** `virtual`

**Answer: D) `virtual`**

**Trace / Why:** Marking a base method `virtual` tells the compiler to resolve the call through the object's vtable at runtime, based on the actual object type behind the pointer/reference — not the declared type of the pointer.

**📘 CONCEPT — Late binding decides by object type, not reference type**
> A non-virtual call is resolved by the *static* (declared) type of the
> pointer/reference at compile time. A virtual call is resolved by the
> *dynamic* (actual) type of the object it points to, checked at runtime. This
> is the single mechanism underlying every "predict the output" question
> involving base-class pointers to derived objects.
>
> **Applies when** a stem has a base-class pointer or reference assigned a
> derived object, and calls a method through it.
>
> **Boundary:** virtual dispatch does **not** apply inside a base class's own
> constructor/destructor — during construction, the object is still "being
> built as a Base," so virtual calls made from the base constructor resolve to
> the base's own version, even if a derived override exists. This is a classic
> trap.

**Wrong traces:** A = `static` methods can't be virtual at all — static binding is the opposite of what's being asked · C = `const` affects mutability of `this`, unrelated to dispatch mechanism · B = `inline` is a compiler hint about function expansion, unrelated to binding.

---

**Q24.** What is the essential difference between function overloading and function overriding? `[Both]` `[Trap]`
- **A)** Overloading happens within the same class with different parameter lists (compile-time); overriding happens in a derived class re-implementing a base method with the same signature (runtime, via virtual dispatch)
- **B)** Overloading and overriding are two names for the same mechanism
- **C)** Overriding requires different parameter lists; overloading requires identical signatures
- **D)** Overloading only exists in Java; overriding only exists in C++

**Answer: A) Overloading happens within the same class with different parameter lists (compile-time); overriding happens in a derived class re-implementing a base method with the same signature (runtime, via virtual dispatch)**

**Trace / Why:** The signature rule is the cleanest test: same name + different parameters, same class → overload. Same name + same signature, different class in a hierarchy → override.

**Wrong traces:** B = conflates two mechanisms with opposite resolution timing · C = exactly backwards — overriding needs an *identical* signature, overloading needs a *different* one · D = both mechanisms exist in both languages.

---

**Q25.** What is a **pure virtual function** in C++? `[C++]` `[Core]`
- **A)** A virtual function declared with `= 0`, forcing any concrete derived class to provide an implementation, and making the class abstract
- **B)** A virtual function with an empty body that still allows the class to be instantiated
- **C)** A function that can never be called
- **D)** A synonym for a static function

**Answer: A) A virtual function declared with `= 0`, forcing any concrete derived class to provide an implementation, and making the class abstract**

**Trace / Why:** `virtual void draw() = 0;` has no base implementation at all — it exists purely as a contract, and any class containing at least one such function cannot be instantiated directly.

**Wrong traces:** B = an empty-bodied virtual function is still instantiable, it's not "pure" in the `=0` sense · C = it *can* be called by a derived override, it just has no default implementation · D = static and virtual are mutually exclusive concepts.

---

**Q26.** Which of the following is **incorrect** about virtual functions? `[C++]` `[Trap]`
- **A)** They are used to achieve runtime polymorphism
- **B)** Each virtual function declaration starts with the `virtual` keyword
- **C)** They are used to hide objects from other classes
- **D)** Late binding / dynamic linkage decides which override runs

**Answer: C) They are used to hide objects from other classes**

**Trace / Why:** Virtual functions govern *which implementation runs* for a call; hiding data from other classes is the job of access modifiers (`private`/`protected`), an entirely separate mechanism.

**Wrong traces:** A, B, D = all correctly describe virtual function behavior — the question asks which is false, and these are true statements, so a student mixing up "dispatch mechanism" with "access control" would wrongly rule these out.

---

**Q27.** `[Code-output]` What does this program print? `[C++]` `[Trap]` `[Code-output]`
```cpp
#include <iostream>
using namespace std;
class Animal {
public:
    void speak() { cout << "Animal sound"; }
};
class Dog : public Animal {
public:
    void speak() { cout << "Bark"; }
};
int main() {
    Animal* a = new Dog();
    a->speak();
    return 0;
}
```
- **A)** `Bark`
- **B)** `Animal sound`
- **C)** `Animal soundBark`
- **D)** Compile-time error

**Answer: B) `Animal sound`**

**Trace / Why:** `speak()` is **not** declared `virtual` in `Animal`, so the compiler resolves the call using the **static type of the pointer** (`Animal*`), not the actual object (`Dog`). Without `virtual`, there is no dynamic dispatch — this is the single most common trap in C++ polymorphism questions.

**Wrong traces:** A = the intuitive-but-wrong answer, assuming dispatch is automatic — it is only automatic when the method is `virtual` · C = fabricates a call to both, which never happens · D = the code compiles fine; slicing/dispatch issues aren't compile errors.

---

**Q28.** `[Code-output]` A base class declares `virtual void greet()`, which internally calls another virtual method `void name()` also declared in the base and overridden in the derived class. A `Base*` points to a `Derived` object; `greet()` is called from the base's own constructor. What gets printed regarding `name()`? `[C++]` `[Trap]` `[Code-output]`
- **A)** The `Derived` class's `name()` override, because the pointer is dynamically `Derived`
- **B)** Undefined output that varies by compiler in a way the standard leaves unspecified
- **C)** A runtime crash, since `Derived` isn't fully constructed
- **D)** The `Base` class's own `name()`, because virtual dispatch does not reach into a not-yet-constructed derived part during the base constructor

**Answer: D) The `Base` class's own `name()`, because virtual dispatch does not reach into a not-yet-constructed derived part during the base constructor**

**Trace / Why:** While the base constructor is executing, the object's dynamic type is still considered `Base` — the derived part hasn't been built yet, so the vtable pointer is set to `Base`'s vtable at that point. Virtual calls made during construction resolve to the currently-executing class's version, not the eventual most-derived one.

**Wrong traces:** A = the natural but wrong assumption that "the pointer says Derived, so Derived runs" — ignoring the construction-time exception to virtual dispatch · C = this is well-defined behavior, not a crash · B = this behavior *is* specified by the standard, not left undefined — the trap is not knowing the rule, not the rule being ambiguous.

---

## Section 5 — Abstract Classes vs Interfaces (Java Focus)

**Q29.** Which is true of a Java **interface** (pre-Java 8 style)? `[Java]` `[Core]`
- **A)** It may declare instance fields with any visibility and provide method bodies
- **B)** It can be instantiated directly with `new`
- **C)** It can only declare abstract method signatures (and implicitly `public static final` constants) — no instance state, no constructor
- **D)** It supports only single inheritance, like classes

**Answer: C) It can only declare abstract method signatures (and implicitly `public static final` constants) — no instance state, no constructor**

**Trace / Why:** Interfaces define a contract only — no object state to initialize, hence no constructor makes sense, and any field is implicitly a constant, not per-instance data.

**Wrong traces:** A = describes an abstract class, not a classical interface · B = an interface has no implementation to run, so it cannot be instantiated · D = interfaces are exactly the mechanism Java uses to support *multiple* inheritance of type.

---

**Q30.** Can an abstract class in Java have a constructor? `[Java]` `[Trap]`
- **A)** No, since abstract classes can't be instantiated, a constructor is pointless
- **B)** Yes — it can't be called with `new` on the abstract class itself, but it still runs via constructor chaining when a concrete subclass is instantiated
- **C)** Only if the class has zero fields
- **D)** Only if declared `final`

**Answer: B) Yes — it can't be called with `new` on the abstract class itself, but it still runs via constructor chaining when a concrete subclass is instantiated**

**Trace / Why:** Every concrete subclass constructor implicitly (or explicitly via `super(...)`) calls the abstract superclass's constructor first, so it's very much used — just never invoked directly by the user.

**Wrong traces:** A = the natural-but-wrong assumption that "can't instantiate" means "can't have a constructor" — it conflates direct instantiation with the constructor-chaining mechanism · C = field count is irrelevant to whether a constructor is legal · D = `abstract` and `final` are mutually exclusive modifiers on a class, so this option describes an impossible class.

---

**Q31.** Which of these differences between abstract classes and interfaces is correct in modern Java (8+)? `[Java]` `[Applied]`
- **A)** An abstract class can have constructors and instance fields; an interface still cannot have either
- **B)** Interfaces can now have constructors just like classes
- **C)** Abstract classes support multiple inheritance; interfaces do not
- **D)** There is no longer any difference between the two

**Answer: A) An abstract class can have constructors and instance fields; an interface still cannot have either**

**Trace / Why:** Java 8 added default and static methods to interfaces, narrowing the *behavior* gap, but interfaces still cannot hold per-instance state or define a constructor — that boundary hasn't moved.

**Wrong traces:** B = false, this never changed · C = exactly backwards — a class extends only one abstract class, but can implement many interfaces · D = default methods narrowed the gap but core differences (state, constructors, single vs multiple inheritance) remain.

---

**Q32.** When should you prefer an abstract class over an interface for a planned future extension of a published API? `[Java]` `[Applied]`
- **A)** Never — interfaces are always superior for evolution
- **B)** When no method needs an implementation at all
- **C)** When you need a class to implement more than one such type simultaneously
- **D)** When you may need to add new methods later, since an abstract class can add a method with a default implementation without breaking every existing subclass

**Answer: D) When you may need to add new methods later, since an abstract class can add a method with a default implementation without breaking every existing subclass**

**Trace / Why:** Before Java 8's default methods, adding an abstract method to a published interface would break every implementer; an abstract class could add a fully-implemented method with zero impact on subclasses. Default methods narrowed this gap but abstract classes still evolve more safely when concrete shared state/logic is involved.

**Wrong traces:** A = overstates the case — evolution safety was historically a real advantage of abstract classes · C = that scenario argues *for* interfaces (multiple implementation), not abstract classes, since a class can only extend one abstract class · B = if nothing needs an implementation, a pure interface is the more natural, lighter-weight choice.

---

## Section 6 — Access Modifiers, Static Members, and Method Hiding

**Q33.** In Java, can a `static` method be overridden? `[Java]` `[Trap]`
- **A)** No — a same-signature static method in a subclass *hides* the superclass version rather than overriding it, since static calls are resolved at compile time by reference type
- **B)** Yes, exactly like an instance method
- **C)** Only if marked `final`
- **D)** Only if the superclass method is `abstract`

**Answer: A) No — a same-signature static method in a subclass *hides* the superclass version rather than overriding it, since static calls are resolved at compile time by reference type**

**Trace / Why:** Static methods belong to the class, not an instance, so there's no object to dispatch on at runtime — the compiler picks the version based on the declared (reference) type at the call site.

**📘 CONCEPT — Hiding (static) vs overriding (instance) resolve by different rules**
> Overriding (instance methods): resolved by the **actual object type** at
> runtime — this is what makes polymorphism work.
> Hiding (static methods): resolved by the **declared reference type** at
> compile time — the object's real type is irrelevant.
>
> **Applies when** a stem has a `Parent ref = new Child();` pattern and calls
> both a static and an instance method with the same name through `ref`.
>
> **Boundary:** `final` is irrelevant to this distinction — it's the `static`
> keyword alone that switches resolution from dynamic to static. A method
> cannot be `static` in one class and instance in a subclass (or vice versa) —
> that's a compile error, not hiding or overriding.

**Wrong traces:** B = the core misconception this question tests · C = `final` prevents overriding entirely, but is unrelated to why static methods hide rather than override · D = `abstract` methods can never be static, so this describes an impossible scenario.

---

**Q34.** `[Code-output]` Given `Parent ref = new Child();` where both classes declare `static void staticMethod()` and both declare (non-static, `@Override`-annotated) `void instanceMethod()`, calling `ref.staticMethod()` then `ref.instanceMethod()` prints: `[Java]` `[Trap]` `[Code-output]`
- **A)** `Child static` then `Child instance`
- **B)** `Parent static` then `Parent instance`
- **C)** `Parent static` then `Child instance`
- **D)** `Child static` then `Parent instance`

**Answer: C) `Parent static` then `Child instance`**

**Trace / Why:** `ref.staticMethod()` is hidden, not overridden — the compiler resolves it using `ref`'s declared type, `Parent`, so `Parent static` runs. `ref.instanceMethod()` is overridden — the JVM resolves it at runtime using the actual object type, `Child`, so `Child instance` runs.

**Wrong traces:** A = assumes both resolve dynamically, ignoring that static methods are hidden not overridden · B = assumes both resolve statically, ignoring that instance methods truly override · D = swaps the two rules entirely.

---

**Q35.** What does it mean for a member to be declared `protected`? `[Both]` `[Core]`
- **A)** Accessible from anywhere in the program
- **B)** Accessible within the same class, subclasses, and (in most implementations) the same package/module, but not from unrelated external code
- **C)** Accessible only within the exact class it is declared in
- **D)** Accessible only from `static` contexts

**Answer: B) Accessible within the same class, subclasses, and (in most implementations) the same package/module, but not from unrelated external code**

**Trace / Why:** `protected` sits between `public` and `private`: it exists specifically to let subclasses (and often same-package code) reach a member that outside, unrelated classes cannot.

**Wrong traces:** A = describes `public` · C = describes `private` · D = access modifiers and the `static` keyword are unrelated axes — a protected member can be static or instance-level.

---

**Q36.** A `static` member variable in a class is best described as: `[Both]` `[Core]`
- **A)** A separate copy created for every object instantiated
- **B)** A variable automatically made `const`
- **C)** A variable that only exists inside constructors
- **D)** A single copy shared across all instances of the class, existing independent of any particular object

**Answer: D) A single copy shared across all instances of the class, existing independent of any particular object**

**Trace / Why:** Static data belongs to the class itself, allocated once regardless of how many (or how few) objects exist — a change through one object is visible through every other object of that class.

**Wrong traces:** A = the opposite is true — that describes ordinary instance fields · C = static fields exist for the program's/class's lifetime, not scoped to a constructor call · B = static and const/final are independent — a static field can be mutable.

---

## Section 7 — Operator Overloading, `this`, and Friend Functions (C++)

**Q37.** What does operator overloading allow a C++ class to do? `[C++]` `[Core]` `[Asked: interviewkickstart]`
- **A)** Change the precedence of built-in operators globally
- **B)** Automatically overload every operator without any code
- **C)** Create entirely new operator symbols not already in the language
- **D)** Give an existing operator (like `+`, `==`, `<<`) a custom meaning when applied to objects of that class

**Answer: D) Give an existing operator (like `+`, `==`, `<<`) a custom meaning when applied to objects of that class**

**Trace / Why:** Overloading `operator+` for a `Complex` class, for instance, lets `c1 + c2` call user-defined logic that adds the real and imaginary parts — it's syntactic sugar over what would otherwise be a named method.

**Wrong traces:** A = operator precedence is fixed by the language grammar and cannot be changed by overloading · C = C++ only allows overloading of existing operator tokens, not invention of new symbols · B = every overload must be explicitly written; nothing happens automatically (aside from a few compiler-generated defaults like copy assignment).

---

**Q38.** Inside a non-static member function, what does the `this` pointer refer to? `[C++]` `[Core]`
- **A)** The base class portion of the object only
- **B)** A pointer to the class definition itself, shared by all objects
- **C)** The current object instance on which the member function was invoked
- **D)** Nothing — `this` does not exist in member functions

**Answer: C) The current object instance on which the member function was invoked**

**Trace / Why:** `this` is an implicit parameter of every non-static member function, holding the address of the specific object the call was made through — it's how the compiler knows *which* object's data to use.

**Wrong traces:** A = `this` refers to the whole object, not just a base subobject slice · B = confuses per-object identity with the shared class metadata/vtable, which `this` is not · D = `this` is precisely defined and available in every non-static member function.

---

**Q39.** Why does Java have no direct equivalent of C++'s `this`-based pointer arithmetic or raw pointer manipulation on objects? `[Java]` `[Applied]`
- **A)** Java has no concept of "the current object" at all
- **B)** Java uses references (managed by the JVM) instead of raw pointers, and disallows pointer arithmetic to preserve memory safety
- **C)** Java objects have no identity
- **D)** Java forbids the `this` keyword entirely

**Answer: B) Java uses references (managed by the JVM) instead of raw pointers, and disallows pointer arithmetic to preserve memory safety**

**Trace / Why:** Java deliberately removed raw pointer arithmetic as a safety measure — `this` still exists and refers to the current object, but you cannot do address math on it the way C++ allows with raw pointers.

**Wrong traces:** A = Java absolutely has a notion of the current object, accessed via `this` · C = every object has identity (`==` compares references/identity) · D = `this` is a valid, commonly used keyword in Java for exactly the same self-reference purpose as C++.

---

**Q40.** What is a `friend` function in C++? `[C++]` `[Applied]`
- **A)** A member function that is automatically virtual
- **B)** A synonym for a constructor
- **C)** A function inherited from a friend class automatically
- **D)** A non-member function explicitly granted access to a class's private and protected members

**Answer: D) A non-member function explicitly granted access to a class's private and protected members**

**Trace / Why:** Declaring `friend void foo(MyClass&);` inside `MyClass` breaks encapsulation deliberately and narrowly — `foo` isn't a member, but the class explicitly opts to trust it with internal access, often used for operator overloading of symmetric operators like `+`.

**Wrong traces:** A = friendship and virtuality are unrelated concepts · C = friendship is not inherited and is not automatic between related classes · B = a friend function is a distinct, separate concept from a constructor.

---

## Section 8 — Casting, Class vs Struct, and Mixed Contrast Questions

**Q41.** What is "upcasting" in OOP? `[Both]` `[Core]`
- **A)** Converting a derived-class reference/pointer to a base-class reference/pointer — always safe
- **B)** Converting a base-class reference/pointer to a derived-class reference/pointer — always safe
- **C)** Converting an `int` to a `float`
- **D)** Deleting an object early

**Answer: A) Converting a derived-class reference/pointer to a base-class reference/pointer — always safe**

**Trace / Why:** Since a `Dog` genuinely *is an* `Animal`, treating it through an `Animal` reference loses no information the base type promised — this conversion is implicit and always safe.

**📘 CONCEPT — Upcasting is safe; downcasting needs a runtime check**
> Upcasting (derived → base) never fails: every derived object satisfies the
> base contract by definition. Downcasting (base → derived) is only safe if
> the object *actually is* that derived type at runtime — hence C++ uses
> `dynamic_cast` (returns null/throws on failure) and Java uses a runtime
> `ClassCastException` if you cast incorrectly.
>
> **Applies when** a stem mentions casting a pointer/reference between a base
> and derived type, or asks which direction is implicit vs which needs an
> explicit/checked cast.
>
> **Boundary:** upcasting is safe even though it can lose access to the
> derived-only members — "safe" means no crash, not "no information hidden."

**Wrong traces:** B = describes downcasting, which is not always safe · C = a primitive numeric conversion, unrelated to class hierarchies · D = an unrelated lifecycle operation, not a casting concept.

---

**Q42.** Downcasting a base-class pointer to a derived-class pointer, when the underlying object is **not actually** that derived type, results in: `[Both]` `[Trap]`
- **A)** A guaranteed compile-time error in both languages
- **B)** The object silently gains the derived class's extra fields, initialized to zero
- **C)** Undefined behavior in C++ with a raw `static_cast`, or (with `dynamic_cast`) a null result / thrown exception; in Java, a thrown `ClassCastException` at runtime
- **D)** No effect at all — the cast is simply ignored

**Answer: C) Undefined behavior in C++ with a raw `static_cast`, or (with `dynamic_cast`) a null result / thrown exception; in Java, a thrown `ClassCastException` at runtime**

**Trace / Why:** Neither language can verify an incorrect downcast at compile time (the actual object type is a runtime property), so both defer the failure to runtime — C++ optionally via the checked `dynamic_cast`, Java always via a checked exception.

**Wrong traces:** A = the whole reason this is dangerous is that it compiles fine — the type mismatch is only visible at runtime · B = casting never allocates or fabricates fields that don't exist in memory · D = an incorrect downcast is a genuine, consequential error, not a no-op.

---

**Q43.** In C++, which of the following is a genuine, meaningful difference between `class` and `struct`? `[C++]` `[Trap]`
- **A)** A `struct` cannot have member functions
- **B)** `struct` supports inheritance but `class` does not
- **C)** A `struct` is always allocated on the stack; a `class` is always on the heap
- **D)** The default access specifier: `private` for `class`, `public` for `struct` (both can otherwise do the same things)

**Answer: D) The default access specifier: `private` for `class`, `public` for `struct` (both can otherwise do the same things)**

**Trace / Why:** In C++ (unlike C), `struct` and `class` are nearly identical — both support member functions, constructors, inheritance, and access control. The *only* built-in difference is the default visibility applied when none is specified.

**Wrong traces:** A = false in C++ — structs can have full member functions, unlike plain C structs · C = storage location depends on how the object is allocated (`new` vs local variable), not on the `class`/`struct` keyword · B = both support inheritance identically in C++.

---

**Q44.** Which of these is the correct definition of "instantiation" in OOP? `[Both]` `[Core]`
- **A)** Defining a class's method bodies
- **B)** Creating an object from a class — allocating memory and running a constructor
- **C)** Compiling source code into an executable
- **D)** Copying a function's bytecode

**Answer: B) Creating an object from a class — allocating memory and running a constructor**

**Trace / Why:** Instantiation is specifically the act of turning a blueprint (class) into a concrete, addressable thing (object) in memory.

**Wrong traces:** A = describes writing the class itself, not creating an object from it · C = a separate build-process step, unrelated to object creation at runtime · D = not a meaningful OOP operation as stated.

---

**Q45.** Why can C++ be used without any OOP features at all, while idiomatic Java code almost always uses classes? `[Both]` `[Applied]`
- **A)** C++ retains full support for C-style procedural programming as a subset of the language; Java requires all code to live inside a class
- **B)** Java doesn't actually support OOP
- **C)** C++ forbids procedural code
- **D)** There is no such difference — both require classes for every line of code

**Answer: A) C++ retains full support for C-style procedural programming as a subset of the language; Java requires all code to live inside a class**

**Trace / Why:** C++ is a strict superset of C, so purely procedural, non-OOP C++ programs are entirely valid. Java has no free-floating functions — every method, including `main`, must be declared inside some class.

**Wrong traces:** B = false, Java is thoroughly class-based and OOP-centric · C = C++ imposes no such restriction; procedural style is fully legal · D = the difference is real and frequently tested.

---

**Q46.** `[Code-output]` What does this Java snippet print? `[Java]` `[Applied]` `[Code-output]`
```java
class Shape {
    void draw() { System.out.print("Shape"); }
}
class Circle extends Shape {
    @Override
    void draw() { System.out.print("Circle"); }
}
class Square extends Shape {
    @Override
    void draw() { System.out.print("Square"); }
}
public class Main {
    public static void main(String[] args) {
        Shape[] shapes = { new Circle(), new Square() };
        for (Shape s : shapes) s.draw();
    }
}
```
- **A)** `ShapeShape`
- **B)** Compile-time error, since `Shape` doesn't know about `Circle` or `Square`
- **C)** `ShapeCircleShapeSquare`
- **D)** `CircleSquare`

**Answer: D) `CircleSquare`**

**Trace / Why:** In Java, every instance method is virtual by default (there's no separate `virtual` keyword needed) — so `s.draw()` always dispatches to the actual runtime type of `s`, printing `Circle` then `Square` as the loop iterates.

**📘 CONCEPT — Java methods are virtual unless `final`, `static`, or `private`**
> Unlike C++, where you opt *in* to dynamic dispatch with `virtual`, Java opts
> you *in* by default — every ordinary instance method uses dynamic dispatch
> unless it's `static` (hidden, not overridden), `private` (not inherited/
> overridable at all), or the class/method is `final` (locked from further
> override).
>
> **Applies when** a Java code-output question uses a base-type array/variable
> holding subclass instances and calls an instance method through it.
>
> **Boundary:** this is a genuine language divergence from C++, where the
> default is static binding and `virtual` is required to opt into dynamic
> dispatch — a learner who assumes Java needs an explicit "virtual" keyword
> will wrongly expect base-class behavior here.

**Wrong traces:** A = assumes static binding by default, which is the C++ non-virtual behavior, not Java's default · C = fabricates extra calls that never happen (no base version runs when overridden) · B = arrays of a base type holding subclass instances are entirely legal and idiomatic in Java.

---

## Section 9 — Generics/Templates and a Genuine Past-Exam "All of the Above"

**Q47.** What is the primary purpose of templates in C++ (generics in Java)? `[Both]` `[Applied]`
- **A)** To let a class or function operate on multiple data types without rewriting the logic for each type
- **B)** To force a class to use only `int` types
- **C)** To make a function run faster than a normal function
- **D)** To automatically generate destructors

**Answer: A) To let a class or function operate on multiple data types without rewriting the logic for each type**

**Trace / Why:** A `template <class T>` in C++ or `class Box<T>` in Java is compiled/checked once and reused for `int`, `String`, a custom class, etc., without duplicating the algorithm for each type.

**Wrong traces:** B = the opposite goal — templates/generics exist specifically to avoid being locked to one type · C = templates are a compile-time code-generation mechanism, not a runtime performance optimization in themselves · D = destructors are unrelated to type parameterization.

---

**Q48.** *(Genuine past-exam item, kept in its original "all of the above" form.)* Which of the following are considered features of Object-Oriented Programming? `[Both]` `[Core]` `[Asked: bank job / university MCQ bank]`
- **A)** Encapsulation, Inheritance, and Polymorphism
- **B)** Message passing between objects
- **C)** Data abstraction
- **D)** All of the above

**Answer: D) All of the above**

**Trace / Why:** This is one of the rare legitimate "all of the above" items — each individual option (A, B, C) is independently a recognized core feature of OOP, and the question is testing whether the learner can recognize the full set rather than isolate just one.

**Wrong traces:** A, B, C individually = each is true but incomplete; picking just one under-selects when the stem is asking about the feature set as a whole, not a single feature.

---

## Section 10 — Extra Code-Output Drills (raising code-output coverage)

**Q49.** `[Code-output]` What does this print? `[C++]` `[Trap]` `[Code-output]`
```cpp
#include <iostream>
using namespace std;
class Animal {
public:
    virtual void sound() { cout << "Animal"; }
};
class Dog : public Animal {
public:
    void sound() override { cout << "Dog"; }
};
int main() {
    Dog d;
    Animal a = d;   // note: object, not pointer/reference
    a.sound();
    return 0;
}
```
- **A)** `Animal` — the `Dog` part is sliced off during the by-value copy into an `Animal` object
- **B)** `Dog` — because `sound()` is virtual
- **C)** A compile-time error, since `Dog` can't be assigned to `Animal`
- **D)** Undefined behavior, since no pointer is involved

**Answer: A) `Animal` — the `Dog` part is sliced off during the by-value copy into an `Animal` object**

**Trace / Why:** `Animal a = d;` copy-constructs a *new, plain* `Animal` object from `d` — only the `Animal` portion of `d`'s memory is copied. There is no pointer or reference involved, so there is nothing for the vtable dispatch to hook into: `a` is genuinely, permanently an `Animal`, regardless of `virtual`.

**📘 CONCEPT — Object slicing**
> Polymorphism in C++ only works through pointers or references to the base
> type. Assigning or passing a derived object *by value* into a variable of
> the base type "slices" away everything beyond the base subobject — `virtual`
> becomes irrelevant because the object genuinely no longer has a `Dog` part.
>
> **Applies when** a stem assigns/passes a derived object into a base-typed
> variable or by-value parameter (not a pointer/reference).
>
> **Boundary:** the exact same code with `Animal& a = d;` or `Animal* a = &d;`
> instead would correctly print `Dog` — the boundary is pointer/reference vs
> plain object, not the presence of `virtual` alone.

**Wrong traces:** B = correct only if `a` held a reference/pointer, not a sliced-copy object · C = this assignment is perfectly legal — it just narrows the object, it doesn't fail to compile · D = slicing is well-defined behavior, not UB; it's just usually not what the programmer intended.

---

**Q50.** `[Code-output]` A base class has a **non-virtual** destructor. A derived class adds a `std::string` member. Code does `Base* p = new Derived(); delete p;`. What happens? `[C++]` `[Trap]` `[Code-output]`
- **A)** Only `~Base()` runs; `~Derived()` (and therefore the `string`'s destructor) never runs — undefined behavior / resource/memory leak
- **B)** Both destructors run in the normal derived-then-base order, no different from the virtual case
- **C)** A compile-time error, since `Base` has no virtual destructor
- **D)** Only `~Derived()` runs, and `~Base()` is skipped

**Answer: A) Only `~Base()` runs; `~Derived()` (and therefore the `string`'s destructor) never runs — undefined behavior / resource/memory leak**

**Trace / Why:** `delete p` on a `Base*` looks at the *static* type of `p` to decide which destructor to call when the destructor isn't `virtual`. Since `~Base()` isn't virtual, the compiler generates a plain call to `~Base()` only — `Derived`'s own destructor (and by extension its `string` member's destructor) is skipped entirely, silently leaking whatever `Derived` owned.

**Wrong traces:** B = describes the *correct*, safe behavior that only happens when the destructor **is** `virtual` — this option is the trap for someone who assumes destructors always cascade correctly regardless of `virtual` · C = this compiles without any diagnostic in ordinary builds — the danger is exactly that it looks fine · D = backwards; if anything runs, it's the base part, not the derived part, since the static type drives dispatch here.

**📘 CONCEPT — Always give a polymorphic base class a virtual destructor**
> If a class is ever going to be deleted through a base pointer, its
> destructor must be `virtual` — otherwise `delete` on a base pointer only
> calls the base's own destructor, skipping every derived member's cleanup.
> This is one of the most common real-world C++ bugs, not just an exam trick.
>
> **Applies when** a stem shows `Base* p = new Derived(); delete p;` and asks
> about correctness or leaks.
>
> **Boundary:** if the class is never deleted polymorphically (only ever
> through its own concrete type), a non-virtual destructor is fine and even
> slightly cheaper — the rule specifically concerns deletion through a base
> pointer/reference.

---

**Q51.** `[Code-output]` Two objects `x` and `y` are created from a class with a `static int count` field, incremented by 1 inside the constructor. After creating both `x` and `y`, what does `System.out.print(x.count)` (Java) show, given `count` started at 0? `[Java]` `[Applied]` `[Code-output]`
- **A)** `0`
- **B)** `1`
- **C)** `2`
- **D)** A compile-time error, since `count` can't be accessed via an instance

**Answer: C) `2`**

**Trace / Why:** `static` fields are shared by every instance of the class, not duplicated per object. Both constructor calls (`x`'s and `y`'s) increment the **same** shared counter, so by the time both objects exist, `count` is 2 — and accessing it via `x.count` is legal syntax (though `ClassName.count` is the idiomatic way), since Java allows static access through an instance reference too.

**Wrong traces:** A = assumes `count` never got incremented, ignoring that both constructors ran · B = assumes each object has its own separate `count` (as if it weren't static), missing that static fields are class-wide, not per-instance · D = accessing a static field through an instance reference (`x.count`) is legal Java, just discouraged in style guides — it isn't a compile error.

---

**Q52.** `[Code-output]` What is the print order for this Java constructor-chaining snippet? `[Java]` `[Applied]` `[Code-output]`
```java
class A {
    A() { System.out.print("A "); }
}
class B extends A {
    B() { System.out.print("B "); }
}
public class Main {
    public static void main(String[] args) {
        new B();
    }
}
```
- **A)** `B A`
- **B)** `A B`
- **C)** `A` only, since `B`'s constructor never runs
- **D)** `B` only, since Java doesn't chain constructors automatically

**Answer: B) `A B`**

**Trace / Why:** Every constructor in Java implicitly calls `super()` as its very first statement unless another constructor call is written explicitly. So `B()`'s body effectively starts with an invisible `super();` call to `A()`, which prints "A " first, and only then does `B()`'s own body print "B ".

**Wrong traces:** A = reverses the implicit super-call order — the parent constructor always completes before the child constructor's own body runs · C = `B`'s constructor absolutely does run — it's the one explicitly invoked by `new B()` · D = Java always implicitly chains to the no-arg superclass constructor unless told otherwise — this is automatic, not something the programmer has to request.

---

**Q53.** `[Code-output]` How many times is the copy constructor invoked in the following, assuming no compiler-level copy elision (a strict trace, as an exam would expect)? `[C++]` `[Trap]` `[Code-output]`
```cpp
class Widget {
public:
    Widget() {}
    Widget(const Widget&) { std::cout << "copy "; }
};
void useByValue(Widget w) {}
int main() {
    Widget original;
    useByValue(original);
    return 0;
}
```
- **A)** Exactly once — when `original` is copied into the by-value parameter `w`
- **B)** Zero times — `Widget` is passed by reference under the hood automatically
- **C)** Twice — once into the parameter, once when the function returns
- **D)** Three times — C++ always copies twice for safety plus once for cleanup

**Answer: A) Exactly once — when `original` is copied into the by-value parameter `w`**

**Trace / Why:** `useByValue(Widget w)` takes its parameter **by value**, so calling it copy-constructs `w` from `original` — one copy. The function returns `void`, so there's no return-value copy to count, and the destructor of `w` runs as `w` goes out of scope, but destruction is not a copy.

**Wrong traces:** B = C++ never silently converts a by-value parameter to a reference — "by value" always means a copy happens, that's the whole meaning of the parameter-passing mode · C = there is nothing to copy on return since the function returns `void` · D = fabricates a rule that doesn't exist in C++'s value semantics.

---

**Q54.** `[Code-output]` Class `C` implements two interfaces, `I1` and `I2`, both of which declare a `default` method `greet()` with different bodies. What happens if `C` does not override `greet()` itself? `[Java]` `[Trap]` `[Code-output]`
- **A)** The compiler picks `I1`'s version automatically, since it's listed first
- **B)** A compile-time error — `C` **must** override `greet()` itself to resolve the conflict between the two default implementations
- **C)** Both versions run, one after another
- **D)** The JVM picks one at random at runtime

**Answer: B) A compile-time error — `C` must override `greet()` itself to resolve the conflict between the two default implementations**

**Trace / Why:** Java has no rule for silently preferring one interface's default method over another's when both apply to the same class — this ambiguity is exactly the "diamond problem" default methods reintroduced, and Java forces the implementing class to break the tie explicitly (typically calling `I1.super.greet()` or `I2.super.greet()` inside its own override).

**📘 CONCEPT — Default methods bring back a mild diamond problem**
> Default methods let interfaces carry behavior, which reopens the door to
> ambiguity when a class implements two interfaces with clashing default
> method signatures. Unlike C++'s virtual-inheritance fix, Java's answer is
> simpler: force the implementing class to resolve it explicitly — there is no
> automatic "first listed wins" rule.
>
> **Applies when** a stem shows a class implementing multiple interfaces that
> each provide a `default` method with the same signature.
>
> **Boundary:** this conflict only arises for **default** methods with
> matching signatures — a plain abstract method declared identically in two
> interfaces causes no conflict at all, since there's no competing
> implementation to choose between.

**Wrong traces:** A = there is no "declaration order wins" rule in Java — this would silently hide a real design conflict, which the language refuses to do · C = only one `greet()` call happens per invocation; there's no mechanism to run both automatically · D = Java resolves this at **compile time** as an error, not at runtime by chance.

---

**Q55.** `[Code-output]` What does this print? `[C++]` `[Applied]` `[Code-output]`
```cpp
#include <iostream>
using namespace std;
class Point {
public:
    int x, y;
    Point(int x, int y) : x(x), y(y) {}
    Point operator+(const Point& other) {
        return Point(x + other.x, y + other.y);
    }
};
int main() {
    Point p1(1, 2), p2(3, 4);
    Point p3 = p1 + p2;
    cout << p3.x << "," << p3.y;
    return 0;
}
```
- **A)** `1,2`
- **B)** `3,4`
- **C)** `4,6`
- **D)** A compile-time error, since `+` cannot be used on custom types

**Answer: C) `4,6`**

**Trace / Why:** `operator+` is overloaded to build a new `Point` whose `x` is the sum of both operands' `x` (1+3=4) and whose `y` is the sum of both operands' `y` (2+4=6). `p1 + p2` therefore calls this custom logic and returns `Point(4, 6)`.

**Wrong traces:** A = just prints `p1`'s own coordinates, ignoring that addition actually ran · B = just prints `p2`'s own coordinates, same mistake in the other direction · D = operator overloading exists precisely to make `+` legal and meaningful for a custom type — this is standard, compiling C++.

---

**Q56.** `[Code-output]` What happens when this Java code is compiled? `[Java]` `[Applied]` `[Code-output]`
```java
class Base {
    final void greet() { System.out.print("Base greet"); }
}
class Derived extends Base {
    void greet() { System.out.print("Derived greet"); }
}
```
- **A)** It compiles and runs fine, printing "Derived greet" whenever `greet()` is called on a `Derived` object
- **B)** A compile-time error, because `final` methods cannot be overridden by a subclass
- **C)** It compiles, but `Derived`'s `greet()` is silently ignored at runtime
- **D)** It compiles only if `Derived`'s method is also marked `final`

**Answer: B) A compile-time error, because `final` methods cannot be overridden by a subclass**

**Trace / Why:** Marking a method `final` in Java is an explicit instruction that no subclass may provide a new implementation — the compiler rejects `Derived`'s attempted `greet()` outright rather than silently allowing or ignoring it.

**Wrong traces:** A = this code doesn't compile at all, so nothing ever runs · C = the failure happens at compile time, not as a silent runtime no-op · D = marking the subclass method `final` too doesn't fix anything — you still cannot override a `final` method regardless of what modifier the attempted override carries.

---

**Q57.** `[Code-output]` What does this print? `[C++]` `[Applied]` `[Code-output]`
```cpp
#include <iostream>
using namespace std;
class Base {
public:
    void greet() { cout << "Base::greet "; }
};
class Derived : public Base {
public:
    void greet() { cout << "Derived::greet "; }
};
int main() {
    Derived d;
    d.Base::greet();
    d.greet();
    return 0;
}
```
- **A)** `Base::greet Derived::greet`
- **B)** `Derived::greet Derived::greet`
- **C)** `Base::greet Base::greet`
- **D)** A compile-time error — a derived object cannot call a base method explicitly

**Answer: A) `Base::greet Derived::greet`**

**Trace / Why:** `d.Base::greet()` uses the scope resolution operator to explicitly force the call to the base version, bypassing whatever `Derived` defines — it prints "Base::greet ". The plain `d.greet()` call, with no qualifier, simply uses ordinary (non-virtual, since `greet` isn't declared `virtual` here) member lookup, which finds `Derived`'s own `greet()` first — it prints "Derived::greet ".

**Wrong traces:** B = ignores that the explicit `Base::` qualifier forces the base version on the first call regardless of the object's actual class · C = ignores that the unqualified second call resolves to the nearest-defined version in `Derived`, since ordinary member lookup starts at the object's own class · D = explicitly qualifying which base class's member to call (`obj.Base::member()`) is valid, common C++ syntax — precisely the same mechanism used to resolve diamond-problem ambiguity (see Q18).

---

## Concept Index

| # | Concept | One-line rule | Questions |
|---|---|---|---|
| 1 | Four pillars are independent | Encapsulation = access, abstraction = complexity, inheritance = reuse-by-hierarchy, polymorphism = call resolution | Q1, Q2 |
| 2 | Object vs class | Class = blueprint (compile-time); object = instance (runtime, own state) | Q3 |
| 3 | Message passing | Objects interact by invoking each other's methods, not touching internals directly | Q4 |
| 4 | OOP history | Alan Kay / Smalltalk = first purely OOP language and origin of the term | Q5 |
| 5 | OOP doesn't guarantee data-duplication-free code | Reuse/modularity/encapsulation are enabled, not automatic | Q6 |
| 6 | Composition vs inheritance | "has-a" → composition; "is-a" → inheritance | Q7 |
| 7 | Constructor purpose | Initializes object state at creation time | Q8 |
| 8 | Constructor overloading | Same name, different parameter lists, resolved at compile time | Q9 |
| 9 | Destructor cannot be overloaded | Exactly one per class, no parameters | Q10 |
| 10 | Construction/destruction mirror order | Base→Derived on construct; Derived→Base on destruct | Q11, Q21 |
| 11 | Copy constructor | Builds a new object as a copy of an existing one of the same class | Q12 |
| 12 | Shallow vs deep copy | Default copy is memberwise; raw pointer members risk double-free | Q13 |
| 13 | No C++-style destructor in Java | Garbage collector reclaims memory non-deterministically; `finalize()` deprecated | Q14 |
| 14 | Five inheritance types (C++) | Single, multilevel, multiple, hierarchical, hybrid | Q15 |
| 15 | Java has no multiple class inheritance | One superclass max; many interfaces allowed | Q16, Q20 |
| 16 | Diamond problem | Shared ancestor reached via two paths → ambiguous member copy | Q17 |
| 17 | Diamond problem fix (C++) | Scope resolution operator, or `virtual` inheritance of the shared base | Q18 |
| 18 | Inheritance access specifiers cap visibility | `protected` inheritance caps public members to protected, etc. | Q19 |
| 19 | Compile-time vs runtime polymorphism | Overloading = compile-time; virtual/override = runtime | Q22, Q24 |
| 20 | `virtual` keyword enables dynamic dispatch (C++) | Non-virtual = static binding by pointer's declared type | Q23, Q27 |
| 21 | Pure virtual functions | `= 0` → no implementation, forces override, makes class abstract | Q25 |
| 22 | Virtual function ≠ access control | Virtual governs dispatch, not visibility/hiding of members | Q26 |
| 23 | Virtual dispatch during construction | Resolves to currently-executing class's version, not final derived type | Q28 |
| 24 | Interfaces (classical) | Abstract method signatures + implicit constants only, no state/constructor | Q29 |
| 25 | Abstract class constructors | Legal, run via chaining from subclass constructors, never called directly | Q30 |
| 26 | Abstract class vs interface (Java 8+) | Default methods narrowed but didn't remove state/constructor gap | Q31 |
| 27 | When to choose abstract class | Safer API evolution when shared state/logic needed | Q32 |
| 28 | Static methods are hidden, not overridden | Resolved by compile-time reference type | Q33, Q34 |
| 29 | `protected` access | Class + subclasses (+ often package), not unrelated external code | Q35 |
| 30 | Static fields | One shared copy per class, independent of instance count | Q36 |
| 31 | Operator overloading | Custom meaning for existing operator tokens on class objects | Q37 |
| 32 | `this` pointer | Refers to the current object instance in a non-static member function | Q38 |
| 33 | Java's memory-safety tradeoff | References + GC instead of raw pointers/pointer arithmetic | Q39 |
| 34 | `friend` functions | Non-member functions explicitly granted private/protected access | Q40 |
| 35 | Upcasting is always safe | Derived → base loses no guaranteed capability | Q41 |
| 36 | Downcasting needs a runtime check | `dynamic_cast`/`ClassCastException` guard against wrong-type casts | Q42 |
| 37 | `class` vs `struct` (C++) | Only real difference is default access specifier | Q43 |
| 38 | Instantiation | Creating an object (memory + constructor run) from a class | Q44 |
| 39 | C++ vs Java structural requirement | C++ allows pure procedural code; Java requires everything inside a class | Q45 |
| 40 | Java methods are virtual by default | No explicit `virtual` keyword needed, unlike C++ | Q46 |
| 41 | Templates / generics | Type-parameterized reuse of one implementation across many types | Q47 |
| 42 | OOP feature set as a whole | Encapsulation + inheritance + polymorphism + abstraction + message passing together define OOP | Q48 |
| 43 | Object slicing | Assigning/passing a derived object by value into a base-typed variable strips the derived part | Q49 |
| 44 | Virtual destructors | A polymorphic base class needs a `virtual` destructor or `delete` via base pointer skips derived cleanup | Q50 |
| 45 | Static fields are shared, not per-instance | Every object's constructor increments the *same* counter | Q51 |
| 46 | Implicit `super()` chaining | Every Java constructor calls the superclass constructor first, even if unwritten | Q52 |
| 47 | By-value parameters always copy | Passing an object by value invokes the copy constructor exactly once, no silent reference optimization | Q53 |
| 48 | Default-method diamond in Java interfaces | Two interfaces with clashing default methods force the implementing class to resolve it explicitly | Q54 |
| 49 | Operator overloading traced numerically | Overloaded `operator+` produces a genuinely new object from component-wise logic | Q55 |
| 50 | `final` methods cannot be overridden | Attempting to override a `final` method is a compile-time error, not a runtime no-op | Q56 |
| 51 | Explicit scope-resolution base call | `obj.Base::method()` forces the base version regardless of what the derived class defines | Q57 |

---

## Answer Key

| Q | Ans | Q | Ans | Q | Ans | Q | Ans |
|---|---|---|---|---|---|---|---|
| 1 | B | 2 | A | 3 | A | 4 | B |
| 5 | C | 6 | C | 7 | C | 8 | A |
| 9 | B | 10 | C | 11 | D | 12 | B |
| 13 | D | 14 | B | 15 | C | 16 | A |
| 17 | A | 18 | C | 19 | B | 20 | D |
| 21 | C | 22 | B | 23 | D | 24 | A |
| 25 | A | 26 | C | 27 | B | 28 | D |
| 29 | C | 30 | B | 31 | A | 32 | D |
| 33 | A | 34 | C | 35 | B | 36 | D |
| 37 | D | 38 | C | 39 | B | 40 | D |
| 41 | A | 42 | C | 43 | D | 44 | B |
| 45 | A | 46 | D | 47 | A | 48 | D |
| 49 | A | 50 | A | 51 | C | 52 | B |
| 53 | A | 54 | B | 55 | C | 56 | B |
| 57 | A | | | | | | |

**Distribution check:** A = 16, B = 15, C = 14, D = 12 (57 questions total) — within about a 15% spread of even, so position-guessing gives little edge.

---

## Coverage Report

| # | Concept | Questions |
|---|---|---|
| 1 | Four pillars are independent (encapsulation/abstraction/inheritance/polymorphism) | Q1, Q2 |
| 2 | Object vs class | Q3 |
| 3 | Message passing | Q4 |
| 4 | OOP history (Alan Kay / Smalltalk) | Q5 |
| 5 | OOP enables but doesn't guarantee good design | Q6 |
| 6 | Composition vs inheritance (has-a vs is-a) | Q7 |
| 7 | Constructor purpose | Q8 |
| 8 | Constructor overloading | Q9 |
| 9 | Destructor cannot be overloaded | Q10 |
| 10 | Construction/destruction mirror order | Q11, Q21 |
| 11 | Copy constructor | Q12 |
| 12 | Shallow vs deep copy | Q13 |
| 13 | No destructor in Java / garbage collection | Q14 |
| 14 | Five inheritance types (C++) | Q15 |
| 15 | No multiple class inheritance in Java | Q16, Q20 |
| 16 | Diamond problem definition | Q17 |
| 17 | Diamond problem resolution (C++) | Q18 |
| 18 | Inheritance access specifiers cap visibility | Q19 |
| 19 | Compile-time vs runtime polymorphism | Q22, Q24 |
| 20 | `virtual` keyword / dynamic dispatch | Q23, Q27 |
| 21 | Pure virtual functions / abstract classes (C++) | Q25 |
| 22 | Virtual ≠ access control | Q26 |
| 23 | Virtual dispatch during construction | Q28 |
| 24 | Classical interfaces (Java) | Q29 |
| 25 | Abstract class constructors | Q30 |
| 26 | Abstract class vs interface (Java 8+) | Q31 |
| 27 | When to prefer abstract class over interface | Q32 |
| 28 | Static method hiding vs overriding | Q33, Q34 |
| 29 | `protected` access modifier | Q35 |
| 30 | Static (class) member fields | Q36 |
| 31 | Operator overloading | Q37 |
| 32 | `this` pointer | Q38 |
| 33 | Java references vs C++ raw pointers | Q39 |
| 34 | `friend` functions (C++) | Q40 |
| 35 | Upcasting (safe) | Q41 |
| 36 | Downcasting (needs runtime check) | Q42 |
| 37 | `class` vs `struct` in C++ | Q43 |
| 38 | Instantiation | Q44 |
| 39 | C++ procedural subset vs Java's class requirement | Q45 |
| 40 | Java methods virtual by default | Q46 |
| 41 | Templates / generics | Q47 |
| 42 | OOP feature set as a whole | Q48 |
| 43 | Object slicing | Q49 |
| 44 | Virtual destructors on polymorphic base classes | Q50 |
| 45 | Static fields are shared across instances | Q51 |
| 46 | Implicit constructor chaining (`super()`) | Q52 |
| 47 | By-value parameters and copy-constructor counting | Q53 |
| 48 | Default-method diamond in Java interfaces | Q54 |
| 49 | Operator overloading traced numerically | Q55 |
| 50 | `final` methods cannot be overridden | Q56 |
| 51 | Explicit scope-resolution base call | Q57 |

All 51 inventory items are covered by at least one question — no gaps.

---

## High-Yield Revision Sheet (night-before page)

1. Encapsulation = access control; abstraction = hiding complexity — don't conflate them.
2. Construction order: base → derived. Destruction order: derived → base. Always mirrored.
3. Default copy = shallow copy. Raw pointer members need a hand-written deep copy (Rule of Three).
4. Java has no destructor — the GC is non-deterministic; `finalize()` is deprecated since Java 9.
5. Five C++ inheritance types: single, multilevel, multiple, hierarchical, hybrid.
6. Java has no multiple class inheritance — only single class inheritance + multiple interfaces.
7. Diamond problem: shared ancestor reached via two paths → ambiguous member copy. Fixed in C++ with `virtual` inheritance or scope resolution.
8. Overloading = compile-time, same class, different parameters. Overriding = runtime, subclass, identical signature.
9. In C++, `virtual` is required to opt into dynamic dispatch — without it, calls resolve by the pointer's static (declared) type.
10. In Java, ordinary instance methods are virtual by default; `static`/`private`/`final` opt out.
11. Static methods in Java are *hidden*, not overridden — resolved by reference type at compile time, not object type.
12. Virtual calls made from inside a base class's own constructor do **not** reach an overriding derived version.
13. Pure virtual function (`= 0`) → class is abstract, cannot be instantiated.
14. Classical interfaces: no instance state, no constructor, only abstract signatures + implicit constants.
15. Abstract classes *can* have constructors — invoked via chaining from a concrete subclass, never called directly.
16. Upcasting (derived→base) is always safe; downcasting needs a checked cast (`dynamic_cast` in C++, or risks `ClassCastException` in Java).
17. `class` vs `struct` in C++: the only real difference is default access (`private` vs `public`).
18. `this` refers to the current object instance inside a non-static member function.
19. `friend` functions get explicit access to private/protected members without being members themselves.
20. Composition ("has-a") ≠ inheritance ("is-a") — use inheritance only for genuine specialization.

---

## Next Step

Run `/mcq OOP --more` for a fresh batch covering deeper edge cases (e.g., virtual destructors in polymorphic base classes, object slicing, `final`/`sealed` classes, SOLID-adjacent design questions), or `/mcq OOP --hard` for trap-tier-only questions to stress-test the concepts you're shakiest on.
