


> Written with [StackEdit](https://stackedit.io/).

# Test Apex Triggers


Before deploying a trigger, write unit tests to perform the actions that fire the trigger and verify expected results.

Testing triggers is same as testing apex class. Only some changes in every test method - 

```java
insert data // first insert test data

// Perform test
Test.startTest(); // start execution of trigger by this inbuilt testing method

// Perform Database.DML(sObjects,false) operation, here we not using DML operations direct but Database class methods and setting second parameter to false i.e allow partial success even error occurs.
Database Result // save database result in DatabaseResult object (Result object is different for every DML operation)

Test.stopTest(); // ending of trigger in testing mode

// Verify trigger results usinh assert and Result methods 
```

We will be testing below trigger for various events - 

```java
trigger temp on Student__c(event){
	Student__c t = Trigger.new[0];
	t.addError('Error');
}
```

## Delete event

`Database.DeleteResult` Class - [Link](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_database_deleteresult.htm#apex_methods_system_database_deleteresult)

### Trigger
```java 
trigger temp on Student__c (after delete,before delete ) {
    System.debug(Trigger.operationType);
    for(Student__c t : Trigger.old){
		t.addError('Error');
    }
}
```
### Test 

```java
@isTest
private class TempTriggerTest {
    @isTest
    static void DeleteTest(){
        Integer m  = 10;
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<m; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        insert lis;
        
        Test.startTest();
        //Database.DeleteResult result = Database.delete(lis[0],false); // for deleting one record
        List<Database.DeleteResult> result = Database.delete(lis,false);// for deleting entire list, for that we have change assert/verify logic accordingly
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

## Insert event 

`Database.SaveResult` Class - [Link](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_database_saveresult.htm)

### Trigger

```java
trigger temp on Student__c (after insert,before insert ) {
    System.debug(Trigger.operationType);
    for(Student__c t : Trigger.new){
		t.addError('Error');
    }
}
```

### Test 

```java
@isTest
private class TempTriggerTest {
    @isTest
    static void InsertTest(){
        Integer m  = 10;
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<m; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        //insert lis;
        
        Test.startTest();
        //Database.SaveResult result = Database.insert(lis[0],false); // for inserting one record
        List<Database.SaveResult> result = Database.insert(lis,false);// for inserting entire list, for that we have change assert/verify logic accordingly
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


## Update event

`Database.SaveResult` Class - [Link](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_database_saveresult.htm)

### Trigger

```java
trigger temp on Student__c (after update,before update ) {
    System.debug(Trigger.operationType);
    for(Student__c t : Trigger.new){
		t.addError('Error');
    }
}
```

### Test

```java
@isTest
private class TempTriggerTest {
    @isTest
    static void UpdateTest(){
        Integer m  = 10;
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<m; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        insert lis;
        
        Test.startTest();
        //Database.InsertResult result = Database.update(lis[0],false); // for updating one record
        List<Database.SaveResult> result = Database.update(lis,false);// for updating entire list, for that we have change assert/verify logic accordingly
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
## Undelete event

`Database.UndeleteResult` Class - [Link](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_database_undeleteresult.htm#apex_methods_system_database_undeleteresult)

### Trigger

```java
trigger temp on Student__c (after undelete ) {
    System.debug(Trigger.operationType);
    for(Student__c t : Trigger.new){
		t.addError('Error');
    }
```

### Test

```java
@isTest
private class TempTriggerTest {
    @isTest
    static void UndeleteTest(){
        Integer m  = 10;
        List<Student__c> lis = new List<Student__c>();
        for(Integer i = 0; i<m; i++){
            lis.add(new Student__c(name = 'Name'+i));
        }
        insert lis;
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















