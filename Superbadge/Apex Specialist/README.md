# [Apex Specialist](https://trailhead.salesforce.com/content/learn/superbadges/superbadge_apex)

## Prerequisites 

### [Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_triggers)

#### [Get Started with Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_triggers/apex_triggers_intro)


Pre-Work:

>Add a checkbox field to the **Account** object:
>
>Field Label: `Match Billing Address`
>
>Field Name: `Match_Billing_Address`
>
>Note: The resulting API Name should be `Match_Billing_Address__c`.

Create an Apex trigger:

>Name: `AccountAddressTrigger`
>
>Object: `Account`
>
>Events: `before insert` and `before update`
>
>Condition: Match Billing Address is `true`
>
>Operation: set the Shipping Postal Code to match the Billing Postal Code

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

**Create a Bulk Apex trigger**

Create a bulkified Apex trigger that adds a follow-up task to an opportunity if its stage is Closed Won. Fire the Apex trigger after inserting or updating an opportunity.
Create an Apex trigger:

>Name: `ClosedOpportunityTrigger`
>
>Object: `Opportunity`
>
>Events: `after insert` and `after update`
>
>Condition: Stage is `Closed Won`
>
>Operation: Create a task:
>
>Subject: `Follow Up Test Task`
, WhatId: `the opportunity ID` (associates the task with the opportunity)
>

Bulkify the Apex trigger so that it can insert or update 200 or more opportunities

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
