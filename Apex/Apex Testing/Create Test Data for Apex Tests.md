


> Written with [StackEdit](https://stackedit.io/).

# Create Test Data for Apex Tests


## Add a Test Utility Class

### Utility class 

**Utility classes** are just like helper classes that consist of reusable(when required many times) methods.  
From triggers, we can call methods in public classes.

### Test data Factory 
Let’s refactor the previous test method by replacing test data creation with a call to a utility class method. First, you need to create the test utility class.

The  `TestDataFactory`  class is a special type of class—it is a public class that is annotated with  `@isTest`  and can be accessed only from a running test. Test utility classes contain methods that can be called by test methods to perform useful tasks, such as setting up test data. Test utility classes are excluded from the org’s code size limit.

### `TestDataFactory` class

```java
@isTest
public class TestDataFactory {
    public static List<Student__c> createStudents(Integer num){
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<num; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        insert lis;
        return lis;
    }
}
```

### Trigger

```java
trigger temp on Student__c (after undelete ) {
    System.debug(Trigger.operationType);
    for(Student__c t : Trigger.new){
		t.addError('Error');
    }
}
```

### Test Class

```java
@isTest
private class TempTriggerTest {
    @isTest
    static void UndeleteTest(){
        List<Student__c> lis = TestDataFactory.createStudents(10); // TestDataFactory Class Method
        delete lis;
        
        Test.startTest();
        // for undeleting one record
        //Database.InsertResult result = Database.undelete([select id from Student__c where isDeleted = true limit 1 ALL ROWS],false); 
        // for undeleting entire list, for that we have change assert/verify logic accordingly
        List<Database.UndeleteResult> result = Database.undelete([select id from Student__c where isDeleted = true ALL ROWS],false);
        Test.stopTest();
        
        for(Integer i=0; i < result.size(); i++){
            System.debug(result[i].getErrors()); 
            System.assert(!result[i].isSuccess()); // negative testing - we checking for fail condition
            System.assert(result[i].getErrors().size()>0);
            System.assertEquals('Error',result[i].getErrors()[0].getMessage());
        }   
    }
}
```















