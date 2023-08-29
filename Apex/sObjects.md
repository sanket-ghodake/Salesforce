


> Written with [StackEdit](https://stackedit.io/).

An sObject variable represents a row of data and can only be declared in Apex using SOAP API name of the object.

For example:
```java
Account a = new Account();
MyCustomObject__c co = new MyCustomObject__c();
```

Similar to SOAP API, Apex allows the use of the generic sObject abstract type to represent any object. The sObject data type can be used in code that processes different types of sObjects.
The `new` operator still requires a concrete sObject type, so all instances are specific sObjects. For example:

```java
sObject s = new Account();
```
You can also use casting between the generic sObject type and the specific sObject type. For example:

```java
// Cast the generic variable s from the example above
// into a specific account and account variable a
Account a = (Account)s;
// The following generates a runtime error
Contact c = (Contact)s;
```
Because sObjects work like objects, you can also have the following:

```java
Object obj = s;
// and
a = (Account)obj;
```

--- 
```java 
public class sObjects {
    public static void main(){
        // sObject is record of an object created in salesforce

        // For standard objects
        Account a1 = new Account();
        a1.Name = 'Sanket';
        System.debug('Account created'+ a1.Name);
        
        // For custom objects 
        Student__c s1 = new Student__c();
        s1.Name = 'Sanket';
        System.debug('Student created'+ s1.Name);
        
        // Generic Objects - 
        sObject g1 = new Account();
        // g1.name = 'Saurabh'; // gives error
        g1.put('Name', 'Saurabh');
        System.debug('Generic object is created'+ g1.get('Name'));
        
        Account a2 = (Account) g1; // casting
        //Contact c1 = (Contact) g1; // gives error
     }
}
```

Every custom object have 4 standard fields -
1. CreatedById
2. LastModifiedById
3. OwnerId
4. Name

So in Apex program and in SOQL, these are treated as standard fields, no need to append `__c` to these fields while accessing them. 
