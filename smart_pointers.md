## Smart Pointers

smart pointers are just like normal pointers but they have some extra metadata.

due to the ownership and borrowing model of rust, references in rust only borrow the data they are pointing to, whereas smart pointers often own the data they are pointing towards.

smart pointers are implemented using `struct`. The difference between a smart pointer and a normal struct is that a smart pointer implements the `Deref` and `Drop` traits. The `Deref` trait allows structs to act as normal pointers and the `Drop` helps us define what happens when a smart pointer instance goes out of scope.

