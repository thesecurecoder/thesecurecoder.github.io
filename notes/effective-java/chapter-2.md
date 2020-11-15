## Creating and Destroying Objects

### Consider static factory methods instead of constructors
**Pros**:
* Unlike constructors, they have names. Useful to differentiate constructors that provide objects of diffierent nature. Ex: BigInteger.probablePrime(int, int, Random).
* Unlike constructors, they are not required to create new object each time they are invoked. Technique similar to Flyweight pattern.
* Classes that maintain strict control over what instances exist at any time: instance-controlled.
* Unlike constructors, they can return an object of any subtype of the return type.