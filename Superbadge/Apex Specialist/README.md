# [Apex Specialist](https://trailhead.salesforce.com/content/learn/superbadges/superbadge_apex)

## Prerequisites 

### [Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_triggers)

#### [Get Started with Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_triggers/apex_triggers_intro)

##### :red_circle: Challenge :
Create an Apex trigger

Create an Apex trigger that sets an account’s Shipping Postal Code to match the Billing Postal Code if the Match Billing Address option is selected. Fire the trigger before inserting an account or updating an account.

**Pre-Work:**  
Add a checkbox field to the Account object:

-   Field Label:  `Match Billing Address`
-   Field Name:  `Match_Billing_Address`  
    Note: The resulting API Name should be  `Match_Billing_Address__c`.

-   Create an Apex trigger:
    -   Name:  `AccountAddressTrigger`
    -   Object:  **Account**
    -   Events: `before insert` and `before update`
    -   Condition: Match Billing Address is true
    -   Operation: set the Shipping Postal Code to match the Billing Postal Code
    
##### :white_check_mark: Solution

```java
trigger AccountAddressTrigger on Account (before insert, before update) {
    for(Account temp: Trigger.new){
        if(temp.Match_Billing_Address__c){
        	temp.ShippingPostalCode = temp.BillingPostalCode;    
        }
    }
}
```

#### [Bulk Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_triggers/apex_triggers_bulk)

##### :red_circle: Challenge :

**Create a Bulk Apex trigger**


Create a Bulk Apex trigger

Create a bulkified Apex trigger that adds a follow-up task to an opportunity if its stage is Closed Won. Fire the Apex trigger after inserting or updating an opportunity.

-   Create an Apex trigger:
    -   Name:  `ClosedOpportunityTrigger`
    -   Object:  **Opportunity**
    -   Events: after insert and after update
    -   Condition: Stage is  `Closed Won`
    -   Operation: Create a task:
        -   `Subject`:  `Follow Up Test Task`
        -   `WhatId`: the opportunity ID (associates the task with the opportunity)
    -   Bulkify the Apex trigger so that it can insert or update 200 or more opportunities

##### :white_check_mark: Solution
```java
trigger ClosedOpportunityTrigger on Opportunity (after insert, after update) {
    List<Task> lis = new List<Task>();
    for(Opportunity temp : Trigger.new){
        if(temp.StageName == 'Closed Won'){
            Task t = new Task(Subject = 'Follow Up Test Task' ,WhatId= temp.Id);
            lis.add(t);
        }
    }
	if(lis.size()>0)
    	insert lis;
}
```

### [Apex Testing](https://trailhead.salesforce.com/content/learn/modules/apex_testing)

#### [Get Started with Apex Unit Tests](https://trailhead.salesforce.com/content/learn/modules/apex_testing/apex_testing_intro)

##### :red_circle: Challenge :

Create a Unit Test for a Simple Apex Class

Create and install a simple Apex class to test if a date is within a proper range, and if not, returns a date that occurs at the end of the month within the range. You'll copy the code for the class from GitHub. Then write unit tests that achieve 100% code coverage.

-   Create an Apex class:
    -   Name:  `VerifyDate`
    -   Code:  **[Copy from GitHub](https://github.com/developerforce/trailhead-code-samples/blob/master/VerifyDate.cls)**
-   Place the unit tests in a separate test class:
    -   Name:  `TestVerifyDate`
    -   Goal: 100% code coverage
-   Run your test class at least once

##### :white_check_mark: Solution

```java
@isTest
public class TestVerifyDate {
    @isTest static void positiveTest(){
        Date d1 = date.newInstance(2023, 8, 13);
        Date d2 = date.newInstance(2023, 8, 30);
        Date result = VerifyDate.CheckDates(d1,d2);
        System.assertEquals(d2,result);
    }
    
    @isTest static void negativeTest(){
        Date d1 = date.newInstance(2023, 8, 13);
        Date d2 = date.newInstance(2023, 9, 30);
        Date result = VerifyDate.CheckDates(d1,d2);
        System.assertEquals(date.newInstance(2023, 8, 31),result);
    }
        
}
```

#### [Test Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_testing/apex_testing_triggers)

##### :red_circle: Challenge :

Create a Unit Test for a Simple Apex Trigger

Create and install a simple Apex trigger which blocks inserts and updates to any contact with a last name of 'INVALIDNAME'. You'll copy the code for the class from GitHub. Then write unit tests that achieve 100% code coverage.

-   Create an Apex trigger on the Contact object
    -   Name:  `RestrictContactByName`
    -   Code:  **[Copy from GitHub](https://github.com/developerforce/trailhead-code-samples/blob/master/RestrictContactByName.cls)**
-   Place the unit tests in a separate test class
    -   Name:  `TestRestrictContactByName`
    -   Goal: 100% test coverage
-   Run your test class at least once

##### :white_check_mark: Solution

```java
@isTest
public class TestRestrictContactByName {
    
    @isTest static void TestContactInsertWithINVALIDNAME(){
    	// Test data setup
        Contact c1 = new Contact( LastName = 'INVALIDNAME');
        // Perform test
        Test.startTest();
        Database.SaveResult result1 = Database.insert(c1, false);
        Test.stopTest();
        // Verify
        System.assert(!result1.isSuccess());
        System.assert(result1.getErrors().size() > 0);
        System.assertEquals('The Last Name "INVALIDNAME" is not allowed for DML',
                             result1.getErrors()[0].getMessage());
     }
    
    @isTest static void TestContactUpdateWithINVALIDNAME(){
    	// Test data setup
        Contact c2 = new Contact( LastName = 'Valid Name');
        insert c2;
        // Perform test
        Test.startTest();
        c2.LastName = 'INVALIDNAME';
        Database.SaveResult result2 = Database.update(c2, false);
        Test.stopTest();
        // Verify
        System.assert(!result2.isSuccess());
        System.assert(result2.getErrors().size() > 0);
        System.assertEquals('The Last Name "INVALIDNAME" is not allowed for DML',
                             result2.getErrors()[0].getMessage());
    }        
}
```

#### [Create Test Data for Apex Tests](https://trailhead.salesforce.com/content/learn/modules/apex_testing/apex_testing_data)

##### :red_circle: Challenge :

Create a Contact Test Factory

Create an Apex class that returns a list of contacts based on two incoming parameters: the number of contacts to generate and the last name. Do not insert the generated contact records into the database.  
  
NOTE: For the purposes of verifying this hands-on challenge, don't specify the @isTest annotation for either the class or the method, even though it's usually required.

-   Create an Apex class in the  `public`  scope
    -   Name:  `RandomContactFactory`  (without the @isTest annotation)
-   Use a Public Static Method to consistently generate contacts with unique first names based on the iterated number in the format Test 1, Test 2 and so on.
    -   Method Name:  `generateRandomContacts`  (without the @isTest annotation)
    -   Parameter 1: An integer that controls the number of contacts being generated with unique first names
    -   Parameter 2: A string containing the last name of the contacts
    -   Return Type:  `List < Contact >`

##### :white_check_mark: Solution

`RandomContactFactory`
```java
public class RandomContactFactory {
    public static List <Contact> generateRandomContacts(Integer n, String lastName){
    	List<Contact> lis = new List<Contact>();
        Contact temp = new Contact();
        String firstName = 'FirstName';
        for(Integer i=0; i<n; i++){
            temp.FirstName = firstName+ i.format();
            temp.LastName = lastName;
            lis.add(temp.clone());
        }  
        return lis;
    }
}
```

