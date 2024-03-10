---
layout: post
title: Delegates in Kotlin pt.1 - Property Delegates
date: 2024-03-10 12:00:00 +0300
description: Learn about property delegates
img: kotlin.jpg 
tags: [Kotlin, Delegates, Design Pattern, Behavioral]
---

Delegates in Kotlin serve as a powerful feature for implementing the Delegate Pattern, which falls under the category of behavioral design patterns. In the context of Kotlin, delegates enable objects to delegate some of their responsibilities or behaviors to other objects, thus facilitating code reuse and promoting a more modular and maintainable codebase.

### It is a behavioral design pattern

As a behavioral design pattern, the Delegate Pattern focuses on how objects interact with each other and how they distribute responsibilities among themselves. Delegates in Kotlin allow you to encapsulate behavior and responsibilities into separate objects, which can then be delegated to by other objects. This separation of concerns helps in managing complexity and promoting a cleaner code structure.

### Property Delegates

Property delegates in Kotlin are a powerful feature that enables you to delegate the implementation of getter and setter methods of a property to another object. This feature is particularly useful when you need custom behavior for accessing or modifying a property, such as lazy initialization, observability, or validation.

Here's how property delegates work:

**Delegate Class:** You create a separate class, known as the delegate class, that defines the behavior for accessing or modifying the property. This delegate class should implement the ReadWriteProperty or ReadOnlyProperty interface, depending on whether the property is mutable or read-only.

**Delegated Property:** In the class where you want to use the property delegate, you declare the property and initialize it using the by keyword followed by an instance of the delegate class.

### Using the already pre-defined class Delegates()

To use the delegates property, we need the delegate class. In Kotlin we already have a built-in class called Delegates() that already give us nice tools to work on and lets check some of them here.

### Delegates.lazy

Is a built-in delegate provided by Kotlin. It lazily initializes the property lazyProperty the first time it's accessed. The lambda passed to lazy is the initializer, which is executed only once when the property is first accessed. Subsequent accesses to lazyProperty return the cached value without re-initializing it.

```kotlin
import kotlin.properties.Delegates

class Example {
    var lazyProperty: String by Delegates.lazy {
        println("Computed!")
        "Hello"
    }
}

fun main() {
    val example = Example()
    println(example.lazyProperty) // Output: Computed! Hello
    println(example.lazyProperty) // Output: Hello (Already computed)
}
```

### Delegates.vetoable

Is a property delegate provided by Kotlin that allows you to veto or reject property assignments based on a condition. This delegate is particularly useful when you want to perform validation or apply constraints to a property's value before allowing it to be set.

Here's how Delegates.vetoable works:

**Delegate Initialization:** You initialize a property with Delegates.vetoable by providing an initial value for the property and a lambda expression that defines the condition for accepting or rejecting new values. The lambda receives two parameters: the property being assigned and the new value being proposed.

**Condition Evaluation:** Whenever the property's value is set, the lambda expression is invoked. If the lambda returns true, the assignment is allowed, and the property is updated with the new value. If the lambda returns false, the assignment is vetoed, and the property remains unchanged.

```kotlin
import kotlin.properties.Delegates

class Example {
    var age: Int by Delegates.vetoable(18) { _, newValue ->
        newValue >= 18
    }
}

fun main() {
    val example = Example()
    
    println(example.age) // Output: 18
    
    example.age = 20
    println(example.age) // Output: 20 (Allowed)
    
    example.age = 15
    println(example.age) // Output: 20 (Rejected, age remains unchanged)
}
```
>In this example, the age property is initialized using Delegates.vetoable with an initial value of 18. The lambda expression checks if >the proposed new value is greater than or equal to 18. If it is, the assignment is allowed; otherwise, it is rejected.
>
>When example.age is set to 20, the condition is satisfied, so the assignment is allowed, and the property is updated accordingly. >However, when example.age is set to 15, the condition is not met, so the assignment is vetoed, and the property remains unchanged.
>
>Delegates.vetoable provides a convenient way to enforce constraints or perform validation on property assignments, ensuring that >property values remain within acceptable bounds or meet specific criteria.

### Delegates.observable

 Is a property delegate provided by Kotlin that allows you to observe changes to a property's value and react to those changes. This delegate is particularly useful when you want to perform actions in response to property assignments, such as updating UI elements, triggering events, or logging changes.

Here's how Delegates.observable works:

**Delegate Initialization:** You initialize a property with Delegates.observable by providing an initial value for the property and a lambda expression that defines the action to be performed when the property's value changes. The lambda receives three parameters: the property being assigned, the old value, and the new value.

**Observation:** Whenever the property's value is set, the lambda expression is invoked with the old value and the new value. You can then perform any desired actions based on these values.

```kotlin
import kotlin.properties.Delegates

class Example {
    var name: String by Delegates.observable("John") { property, oldValue, newValue ->
        println("Property ${property.name} changed from $oldValue to $newValue")
    }
}

fun main() {
    val example = Example()
    
    example.name = "Alice"
    // Output: Property name changed from John to Alice
    
    example.name = "Bob"
    // Output: Property name changed from Alice to Bob
}

```
>In this example, the name property is initialized using Delegates.observable with an initial value of "John". The lambda expression >prints a message indicating the change in the property's value whenever it's assigned a new value.
>
>When example.name is set to "Alice", the observable delegate invokes the lambda expression with the old value "John" and the new value >"Alice", printing the corresponding message. Similarly, when example.name is set to "Bob", the lambda is invoked again with the old >value "Alice" and the new value "Bob", printing another message.
>
>Delegates.observable provides a convenient way to react to changes in property values, enabling you to encapsulate side effects or >perform additional actions whenever properties are modified. This delegate is commonly used in scenarios where you need to maintain >consistency between different parts of your codebase or synchronize state changes with external components.

### Conclusion

Property delegates in Kotlin provide a clean and concise way to implement common patterns, such as lazy initialization, observable properties, and more. Additionally, Kotlin allows you to define custom property delegates tailored to your specific requirements, making it a versatile feature for managing properties in your codebase.

Please check my pt.2 about making a custom property delegates and take it to the next level.