## If you update or delete a record in its before trigger, or delete a record in its after trigger, you will receive a runtime  error. This includes both direct and indirect operations. 

### If you update or delete a record in its before trigger - it gives error. 

```java
trigger temp on Account (before update,before insert,before delete ) {
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
### If you update or delete a record in its after trigger - it's fine. No error

```java
trigger temp on Account (after update,after insert,after delete ) {
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
