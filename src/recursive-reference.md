# Recursive reference

Recursive reference is a primitive intended to traverse tree-like structures.
`RecRef<'a, T>` is a type that behaves like a `&'a mut T`. However, `RecRef<'a, T>` allows pushing a new `&'a mut T` derived from the current value into it, which later can be popped, returning to the original reference.

In order to ensure safety, `RecRef<'a, T>` must ensure that only the "top" or "current" reference may be used. In general, to satisfy the [single-mutator principle], Only one of the reference can be usable at any given time.

In some senses, this is already possible with a regular `&'a mut T`. It's possible to either:
* Overwrite the current reference with a new reference. However, returning to the original reference is impossible.
* Storing a derived reference in another variable. However, that variable must have a smaller lifetime, and so this can only be done as many times as you have variables.
* Pass a derived reference into a recursive call. This works since each recursive call has a different lifetime while "in the code" being the same variable. However, this necessarilly couples the program's control flow with the structure of the data.

## Implementation details


