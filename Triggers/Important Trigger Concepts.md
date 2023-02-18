## If you update or delete a record in its before trigger, or delete a record in its after trigger, you will receive a runtime  error. This includes both direct and indirect operations. [:link:](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers.htm#:~:text=Additionally%2C%20if%20you%20update%20or%20delete%20a,and%20you%20will%20receive%20a%20runtime%20error.)






## We can modify records by which trigger occurs in it's before events but not in after events. (No need of DML statements for this records as they implicitely saved in trigger flow). We can't modify other records (Other than records which fires the trigger) in before events using DML statements (Only way to update other records). But we can modify other records in after events. 
*Note -This statement is specific to same event and same operation in trigger. DML statement is performed inside same event. Events and operation to be performed are same here, example - updating record in update event, deleteing record in delete event, inseting record in isert event*

- Record names used here are - `Sanket number` format

### Update record in trigger -

#### :x: If you update other record in its before update trigger - it gives error. 

```java
trigger temp on Account (before update) {
    // assuming that single record initialize the trigger
    if(Trigger.isBefore){
        Account record = new Account();
        System.debug('Before Trigger - ');
        record = Trigger.new[0];
        System.debug('Record which causes trigger -');
        System.debug(record);
        List<Account> accLis = [select Name,ID from Account where Name ='Sanket' or Name ='Sanket 1'];
        System.debug('Existing records in database');
        
        for(Account temp: accLis){
            System.debug(temp);
            temp.Name+= 'Updated';
        }
        update accLis; // Direct DML Operation
    }   
}
```
> ![DEBUG LOG](.//update%20before%20trigger.png)
#### :white_check_mark: If you update other record in its after update trigger - it's fine. No error

```java
trigger temp on Account (after update ) {
    // assuming that single record initialize the trigger
    if(Trigger.isAfter){
        Account record = new Account();
        System.debug('After Trigger - ');
        record = Trigger.new[0];
        System.debug('Record which causes trigger -');
        System.debug(record);
        List<Account> accLis = [select Name,ID from Account where Name ='Sanket' or Name ='Sanket 1'];
        System.debug('Existing records in database');
        
        for(Account temp: accLis){
            System.debug(temp);
            temp.Name+= 'Updated';
        }
        update accLis; // Direct DML Operation
    }   
}
```
> ![DEBUG LOG](.//update%20after%20trigger.png)

### Insert record in trigger - 

#### :x: If you insert other record in its before insert trigger - it gives error. 

```java
trigger temp on Account (before insert ) {
    // assuming that single record initialize the trigger
    if(Trigger.isBefore){
        Account record = new Account();
        System.debug('Before Trigger - ');
        record = Trigger.new[0];
        System.debug('Record which causes trigger -');
        System.debug(record);
        //System.debug('Record is updated -');
        //record.Name+= ' Updated';
        //System.debug(record);
        Account a = new Account(Name = 'Sanket 2');
        List<Account> t = [ select Name,ID from Account where Name = 'Sanket 2'];
        System.debug('Is Sanket 2 present'+ t.size());
        if(t.size() == 0)
        	insert a;
    }   
}
```
> Infinite Recursion occurs here -
> ![DEBUG LOG](.//insert%20before%20trigger.png)

#### :white_check_mark: If you insert other record in its after insert trigger - it's fine. No error

```java
trigger temp on Account (after insert ) {
    // assuming that single record initialize the trigger
    if(Trigger.isAfter){
        Account record = new Account();
        System.debug('After Trigger - ');
        record = Trigger.new[0];
        System.debug('Record which causes trigger -');
        System.debug(record);
        //System.debug('Record is updated -');
        //record.Name+= ' Updated';
        //System.debug(record);
        Account a = new Account(Name = 'Sanket 2');
        List<Account> t = [ select Name,ID from Account where Name = 'Sanket 2'];
        System.debug('Is Sanket 2 present'+ t.size());
        if(t.size() == 0)
        	insert a;
    }   
}
```
> ![DEBUG LOG](.//insert%20after%20trigger.png)


### Delete record in trigger - 

#### :x: If you delete other record in its before delete trigger - it gives error. 

```java
trigger temp on Account (before delete ) {
    // assuming that single record initialize the trigger
    if(Trigger.isBefore){
        Account record = new Account();
        System.debug('Before Trigger - ');
        //record = Trigger.new[0]; 
        //System.debug(record);
        System.debug('Size of Trigger.old in delete event -'+ Trigger.old.size());
        Id record_Id = Trigger.old[0].Id; // Trigger occurs due to deletion of this record.
        System.debug('Trigger occurs due to deletion of this record - '+ record_Id +' '+ Trigger.old[0].Name);
        System.debug('Record which causes trigger - we can\'t access trigger.new context variable in delete event. But we can query for deleted records in Apex');
        
        // delete other record in before delete record
        List<Account> t = [ select Name,ID from Account where Name = 'Sanket 1'];
        System.debug('Is Sanket 1 present'+ t.size());
        if(t.size()!=0)
            delete t;         
    }   
}
```
> Infinite Recursion occurs here -
> ![DEBUG LOG](.//delete%20before%20trigger.png)

#### :white_check_mark: If you delete other record in its after delete trigger - it's fine. No error

```java
trigger temp on Account (after delete ) {
    // assuming that single record initialize the trigger
    if(Trigger.isAfter){
        Account record = new Account();
        System.debug('After Trigger - ');
        //record = Trigger.new[0]; 
        //System.debug(record);
        System.debug('Size of Trigger.old in delete event -'+ Trigger.old.size());
        Id record_Id = Trigger.old[0].Id; // Trigger occurs due to deletion of this record.
        System.debug('Trigger occurs due to deletion of this record - '+ record_Id +' '+ Trigger.old[0].Name);
        System.debug('Record which causes trigger - we can\'t access trigger.new context variable in delete event. But we can query for deleted records in Apex');
        
        // delete other record in after delete record
        List<Account> t = [ select Name,ID from Account where Name = 'Sanket 1'];
        System.debug('Is Sanket 1 present'+ t.size());
        if(t.size()!=0)
            delete t;       
        
    }   
}
```
> ![DEBUG LOG](.//delete%20after%20trigger.png)




