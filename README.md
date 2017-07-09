This package acts as an interface onto a custom assertion libraries that unit tests are able to make use of.  This
interface only concerns itself with reporting against assertion results rather than composition of assertions.

Although not defined anywhere, the signature for `Promise` is described below:

```haskell
interface Promise a b where
    then :: (b -> Promise c d) * (a -> Promise c d) -> Promise c d
    catch :: (a -> Promise c d) -> Promise c d
```

Some commentary on this signature:

* A `Promise` as a parametric type accepting an error and success type.
* The `then` function method accepts a single argument made up of a tuple - the tuple's first element performs a 
  transformation on the promise's success whilst the second element performs the transformation on the error.
* There are some ancillary functions that are used to construct a `Promise` and operate over a `Promise` instance: 
  `Promise.resolve`, `Promise.reject`, `Promise.all` and `Promise.race`.  These functions are not included in the
  interface's signature.
  
Having defined `Promise` it is now possible to define `Assertion`.

```haskell
interface Assertion extends Promise { fileName :: String, lineNumber :: Int, message :: String } ()
```

Some commentary on this signature:

* The notation `{ fileName :: String, lineNumber :: Int, message :: String }` describes a record type - 3 named fields with
  their types.
* The notation `()` for the promise's success type defines it as an any type.
* As described earlier this signature describes how the testing framework is able to interpret the result of an 
  assertion and, in the event that an assertion has failed, provide a diagnostic as to where the failure occurred.  The
  construction of an assertion is the responsibility of an assertion package.
 
