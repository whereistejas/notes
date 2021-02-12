## traits

traits are like the `interfaces` of Rust.

the type of an object defines what kinds of data it can hold, and the behaviour of that type determines what kind of operations/actions we can perform on objects of a particular type.

all of this is just a fancy way of saying, if I have a variable then, which methods I can use it with is determined by the type of the variable.

if I have two variables of type integer and floating-point, then I can perform an addition operation on both of them. the two types, integer and floating-point, both share a trait that allows them to be added. the trait here being "addition".

thus, traits allow us to group shared behaviour across various different types or even a generic type.

