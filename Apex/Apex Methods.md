


> Written with [StackEdit](https://stackedit.io/).

### Optional parameters, or default parameters :

[Apex doesn't have any concept of optional parameters.](https://salesforce.stackexchange.com/questions/16970/set-default-value-for-method-inputs#:~:text=Apex%20doesn%27t%20have%20any%20concept%20of%20optional%20parameters.)

In this case the best approach is going to be to overload the method, as Apex doesn't have any concept of optional parameters.

So to illustrate, your class might look like this:

```java
public void myMethod(String input1, String input2) {
    System.debug(input1);
    System.debug(input2);
}

public void myMethod(String input1) {
    myMethod(input1, 'DEFAULT VALUE STRING');
}
```
