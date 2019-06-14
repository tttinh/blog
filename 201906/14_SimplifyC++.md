# C++11 features you must really use

[Home](../README.md)

## auto

C++11 introduces type inference capability using the auto keyword, which means that the compiler infers the type of a variable at the point of declaration. `auto` is now a sort of placeholder for a type, telling the compiler it has to deduce the actual type of a variable that is being declared from its initializer. It can be used when declaring variables in different scopes such as namespaces, blocks or initialization statement of for loops. Using the auto keyword permits to spend less time having to write out things the compiler already knows.

## nullptr

The constant 0 has had the double role of constant integer and null pointer constant. C++11 corrects this by introducing a new keyword to serve as a distinguished null pointer constant: `nullptr`

## static_assert

C++11 introduces a new way to test assertions at compile-time, using the new keyword `static_assert`, this feature is very useful to add conditions to the template parameters. `static_assert` performs an assertion check at compile-time. If the assertion is true, nothing happens. If the assertion is false, the compiler displays the specified error message.

`static_assert` becomes more useful when used together with type traits. These are a series of classes that provide information about types at compile time. They are available in the `<type_traits>` header. There are several categories of classes in this header: helper classes, for creating compile-time constants, type traits classes, to get type information at compile time, and type transformation classes, for getting new types by applying transformation on existing types.

## default and delete identifier

In C++03, the compiler provides, for classes that do not provide them for themselves, a default constructor, a copy constructor, a copy assignment operator (operator=), and a destructor. The programmer can override these defaults by defining custom versions.

However, there is very little control over the creation of these defaults. Making a class inherently non-copyable, for example, requires declaring a private copy constructor and copy assignment operator and not defining them. In C++11, certain features can be explicitly disabled.

## override and final identifier

In C++03, it is possible to accidentally create a new virtual function, when one intended to override a base class function. Two new special identifiers (not keywords) have been added: `override`, to indicate that a method is supposed to be an override of a virtual method in a base class, and `final`, to indicate that a derived class shall not override a virtual method.

The `override` special identifier means that the compiler will check the **base class(es)** to see if there is a virtual function with this exact signature. And if there is, the compiler will indicate an error.

The `final` special identifier means that the compiler will check the **derived class(es)** to see if there is a virtual function with this exact signature. And if there is, the compiler will indicate an error.

## Smart pointers

The smart pointer is not a new concept, many libraries implemented it many years ago, what’s new is their standardization, and no need anymore to use an external library to work with smart pointers.

- `unique_ptr`: should be used when ownership of a memory resource does not have to be shared (it doesn't have a copy constructor), but it can be transferred to another unique_ptr (move constructor exists).
- `shared_ptr`: should be used when ownership of a memory resource should be shared (hence the name).
- `weak_ptr`: holds a reference to an object managed by a shared_ptr, but does not contribute to the reference count; it is used to break dependency cycles (think of a tree where the parent holds an owning reference (shared_ptr) to its children, but the children also must hold a reference to the parent; if this second reference was also an owning one, a cycle would be created and no object would ever be released).

## Lambdas

Anonymous functions, called lambda, is a powerful feature borrowed from functional programming, that in turned enabled other features or powered libraries. You can use lambdas wherever a function object or a functor or a `std::function` is expected.

## Strongly-typed enums

"Traditional" enums in C++ have some drawbacks: they export their enumerators in the surrounding scope (which can lead to name collisions, if two different enums in the same have scope define enumerators with the same name), they are implicitly converted to integral types and cannot have a user-specified underlying type.

These issues have been fixed in C++ 11 with the introduction of a new category of enums, called strongly-typed enums. They are specified with the enum class keywords. They no longer export their enumerators in the surrounding scope, are no longer implicitly converted to integral types and can have a user-specified underlying type (a feature also added for traditional enums).

## Variadic template

The variadic template is a template, which can take an arbitrary number of template arguments of any type. Both the classes & functions can be variadic.

## Range-based for loops

C++11 augmented the for statement to support the “foreach” paradigm of iterating over collections. it makes the code more simple and cleaner.

## Initializer lists

In C++03 Initializer lists concern only arrays, in C++11 are not just for arrays anymore. The mechanism for accepting a `{}`-list is a function (often a constructor) accepting an argument of type `std::initializer_list<T>`.
