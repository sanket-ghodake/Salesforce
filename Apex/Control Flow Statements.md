


> Written with [StackEdit](https://stackedit.io/).

## Control Flow Statements

### Conditional (If-Else) Statements 


The conditional statement in Apex works similarly to Java.

```java
if ([Boolean_condition])
// Statement 1
else
// Statement 2
```
The `else` portion is always optional, and always groups with the closest `if`. 

### Switch Statements

Apex provides a switch statement that tests whether an expression matches one of several values and branches accordingly.
The syntax is:
```java
switch on expression {
when value1 { // when block 1
// code block 1
}
when value2 { // when block 2
// code block 2
}
when value3 { // when block 3
// code block 3
}
when else { // default block, optional
// code block 4
}
}
```

The when value can be a single value, multiple values, or sObject types. For example:

```java
when value1 {
}
when value2, value3 {
}
when TypeName VariableName {
}
```

The switch statement evaluates the expression and executes the code block for the matching when value. If no value matches, the
when else code block is executed. If there isn’t a when else block, no action is taken.
Note: There is no fall-through. After the code block is executed, the switch statement exits.
Apex switch statement expressions can be one of the following types.
• Integer
• Long
• sObject
• String
• Enum

When Blocks
Each when block has a value that the expression is matched against. These values can take one of the following forms.
> when literal {} (a when block can have multiple, comma-separated literal clauses)
> 
>when SObjectType identifier {}
>
>when enum_value {}
>
>The value null is a legal value for all types.

### Loops

Apex supports five types of procedural loops.
These types of procedural loops are supported:

```java
• do {statement} while (Boolean_condition);
• while (Boolean_condition) statement;
• for (initialization; Boolean_exit_condition; increment) statement;
• for (variable : array_or_set) statement;
• for (variable : [inline_soql_query]) statement;
All loops allow for loop control structures:
• break; exits the entire loop
• continue; skips to the next iteration of the loop
```
