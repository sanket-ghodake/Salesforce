In Salesforce Apex, you can convert a String to an Integer using the `Integer.valueOf(String)` method or the `Integer.parseInt(String)` method. Here's how you can do it:

**Using `Integer.valueOf(String)`:**

```apex
String strNumber = '123'; // Replace '123' with your String
Integer intValue = Integer.valueOf(strNumber);
```

**Using `Integer.parseInt(String)`:**

```apex
String strNumber = '123'; // Replace '123' with your String
Integer intValue = Integer.parseInt(strNumber);
```

Both of these methods will parse the input String and return an Integer value. If the input String cannot be converted to an Integer (e.g., if it contains non-numeric characters), it will throw a `NumberFormatException`. Therefore, it's essential to handle exceptions if there's a possibility of invalid input.
