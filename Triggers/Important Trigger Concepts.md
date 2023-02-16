## If you update or delete a record in its before trigger, or delete a record in its after trigger, you will receive a runtime  error. This includes both direct and indirect operations. 

### Update record in trigger -

#### If you update a record in its before update trigger - it gives error. 

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
#### If you update a record in its after update trigger - it's fine. No error

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

#### If you insert a record in its before insert trigger - it gives error. 

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

#### If you insert a record in its after insert trigger - it's fine. No error

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

