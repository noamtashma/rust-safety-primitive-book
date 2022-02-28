# Rc

[std]

`Rc` is primitive that allows shared ownership of a value.
Ordinarily, ownership must always be unique. Therefore, `Rc` must deal with the problems that unique ownership solves.

* Single mutation principle - only one actor may hold a unique reference to something at any point in time. The solution is thaT `Rc` doesn't expose unique reference to its contents. Therefore, to be useful, it's usually combined with `RefCell` to regain unique reference into the object.
* Dropping - No value can be dropped while pointers to it are still alive. Therefore, `Rc` must track whether any owners to the same object are still alive.

Notably, `Rc` doesn't need to ensure that the values do get dropped for safety. It only drops the elements in the common case. However, it is possible to make a memory leak using `Rc` by creating a loop.

(Historical note?)

This means that `&'static T` is a safe "variant" of `Rc<T>`, with `Rc::new(val)` being replaced by `Box::leak(Box::new(val))`.

However, `Rc` does drop its values usually, and also exposes `Weak` in order to allow cycle-breaking.

## Implementation details

`Rc` stores an allocation, in which there is an `RcBox`, containing a reference counter and the value. When an `Rc` is cloned, the reference counter is incremented and 

# Arc