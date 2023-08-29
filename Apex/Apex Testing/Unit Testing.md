


> Written with [StackEdit](https://stackedit.io/).

#  Apex Unit Tests

The Apex testing framework enables you to write and execute tests for your Apex classes and triggers on the Lightning Platform.

## Code Coverage Requirement for Deployment

Before you can deploy your code or package it for the Lightning Platform AppExchange, at least 75% of Apex code must be covered by tests, and all those tests must pass. In addition, each trigger must have some coverage.

Before each major service upgrade, Salesforce runs all Apex tests on your behalf through a process called Apex Hammer. The Hammer process runs in the current version and next release and compares the test results. This process ensures that the behavior in your custom code hasn’t been altered as a result of service upgrades.

## Test Method Syntax

Test methods are defined using the  `@isTest`  annotation and have the following syntax:

```java
@isTest static void testName() {
    // code_block
}
```

The visibility of a test method doesn’t matter, so declaring a test method as public or private doesn’t make a difference as the testing framework is always able to access test methods.

Test methods must be defined in test classes, which are classes annotated with  `@isTest`. This sample class shows a definition of a test class with one test method.

```java
@isTest
private class MyTestClass {
    @isTest static void myTest() {
        // code_block
    }
}
```

Test classes can be either private or public. If you’re using a test class for unit testing only, declare it as private. Public test classes are typically used for test data factory classes, which are covered later.

The verifications are done by calling the `System.assertEquals()` method, which takes two parameters: the first is the expected value, and the second is the actual value.

We can use `System.assert` methods or methods from `Assert` class, to validate test results. 

The `msg`  parameter in some of the methods takes message input which will be shown with an `Assertion Failed` statement. 
(If test passed no message will be shown.) [Assert Class](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_class_System_Assert.htm)


## Create and Execute a Test Suite

A test suite is a collection of Apex test classes that you run together. For example, create a suite of tests that you run every time you prepare for a deployment or Salesforce releases a new version

## Create Test Data

Salesforce records that are created in test methods aren’t committed to the database. They’re rolled back when the test finishes execution. This rollback behavior is handy for testing because you don’t have to clean up your test data after the test executes.

**By default, Apex tests don’t have access to pre-existing data in the org,** except for access to setup and metadata objects, such as the User or Profile objects. Set up test data for your tests.

### Note
Even though it is not a best practice to do so, there are times when a test method needs access to pre-existing data. To access org data, annotate the test method with `@isTest(SeeAllData=true)`. The test method examples in this unit don’t access org data and therefore don’t use the `SeeAllData` parameter.


## Example -

### Class - 

```java
public class StudentManagement {
    public static Integer StudentCount(){
        return [select count() from Student__c];
    }
    public static String StudentByRollNumber(Integer n){
        Student__c temp = [select Name from Student__c where Roll_Number__c = :n];
        return temp.Name;
    }
    
}
```

### Test Class - 

```java
@isTest
private class StudentManagementTest {
	@isTest
    static void StudentCountTest(){
	    // we have to create test data every time we run the test
        Integer m  = 10;
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<m; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        insert lis; // if we don't create data we get null everytime
		
		// testing methods based on above data
        Integer n = StudentManagement.StudentCount();
        System.debug(n);
        System.assertEquals(m, n); // this line decide test fail pass
    }
    
    @isTest
    static void StudentByRollNumberTest(){
        Integer m  = 10;
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<m; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        insert lis;
        String name  = StudentManagement.StudentByRollNumber(5);
        System.assertEquals('Name4', name);
    }
    
}
```


## Note 

- Starting with Salesforce API 28.0, test methods can no longer reside in non-test classes and must be part of classes annotated with `IsTest`. 
- Test methods can’t be used to test Web service callouts. Instead, use mock callouts. 
- You can’t send email messages from a test method.
- Since test methods don’t commit data created in the test, you don’t have to delete test data upon completion.
- If the value of a static member variable in a test class is changed in a testSetup or test method, the new value isn’t preserved. Other test methods in this class get the original value of the static member variable. This behavior also applies when the static member variable is defined in another class and accessed in test methods.
- For some sObjects that have fields with unique constraints, inserting duplicate sObject records results in an error. 
- Calls to `System.debug` aren’t counted as part of Apex code coverage.  
-  Test methods and test classes aren’t counted as part of Apex code coverage.
