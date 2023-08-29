


> Written with [StackEdit](https://stackedit.io/).

## SOQL - Salesforce Object Query Language

### SOQL in Query Editor

It is used to get the records out of the database into an apex program. 

The records we get in Apex program using SOQL query are in form of sObjects.

Note: 

1. No `;` at the end of SOQL query in Query editor - Developer console
2. No `*` operator, like SQL
3.  `!=null` to chek not null values, and ` =null` to check for null values
4. SOQL does not support regular expressions
5.  For comparios use `=` and not `==` 
6. We can't query deleted records in Query Editor in Developer Console
7.  Case insensitive search 
8. `COUNT()` cannot be used with `GROUP BY` - use `COUNT(field_name)` instead
9.  There is no `JOIN` in SOQL 
10. `LIMIT` is last clause in SOQL query  running in Query Editor while `ALL ROWS` is last clause in SOQL quey in Apex program.
11. Every custom object have 4 standard fields -CreatedById, LastModifiedById, OwnerId, Name
So in Apex program and in SOQL, these are treated as standard fields, no need to append `__c` to these fields while accessing them.
12. semi join sub selects can only query id fields, cannot use: 'name' [Click here](#point_number_12)
13. only root queries support aggregate expressions, we can not use aggregate function inside child query [Click here](#point_number_13) 
14. Field to field comparison in an SOQL WHERE clause  [Click here](#point_number_14) 
15. We cant get String for object 
 Example - `select BillingAddress from Account` will show Object as output. We have to fetch individual fields like `BillingCity`, `BillingCountry`

```sql 
// Basic Syntax 
select {field API name list} from {Object API name}

// Querying records from Standard objects
select Name from Account

// Querying records from Custom objects
select Name from Student__c

//Querying multiple fields 
select Name, Email from Account

// Where
select Name, Salary from Acount 
where Salary > 100

// OR | AND
select Name, Salary from Acount 
where (Salary > 100 AND Salary <500) OR Name like 'S%'

// IN
select Name from Account 
where Name in ('University of Arizona','sForce')

// NOT
select Name from Account 
where Name not in ('University of Arizona','sForce')

// LIKE
// Wild Cards - 
// 	1. % - 0 or multiple characters
// 	2. _ - single character
select Name from Account 
where Name like 'S%'

// Aggregate Functions 
// 	SUM(field_name), MAX(field_name), MIN(field_name), COUNT(), COUNT(field_name), COUNT_DISTINCT(field_name), AVG(field_name)

// ORDER BY
select name, AnnualRevenue
from Account 
order by name, AnnualRevenue desc 

// GROUP BY 
select type, count(name), max(AnnualRevenue)
from Account 
group by type

// Below query gives error
select type, count(), max(AnnualRevenue)
from Account 
group by type
// COUNT() cannot be used with GROUP BY - use COUNT(Id) instead

// LIMIT 
select name
from Account 
order by name desc
limit 5

//FOR UPDATE:
// We can use FOR UPDATE only in Apex program itself.
// At that time only we can update those records, for the time being those reocrds locked for other users.

// ALL ROWS and isDeleted
// We can use ALL ROWS only in Apex program itself to fetch deleted records

```
 <a id="point_number_12">semi join sub selects can only query id fields, cannot use: ‘name’ :point_down:</a>
```SQL
select name from student__c
where name not in (select name from course__C)
// Above query gives error - semi join sub selects can only query id fields, cannot use: 'name'
// As SOQL query by default returns ID field, we can't write sub query to fetch other fields.
```
<a id="point_number_13">only root queries support aggregate expressions :point_down:</a>
```SQL 
// This query give above error
select Id, name, (select count(name) from contacts) from Account
```


### SOQL in Apex
```java
public class SOQL {
    
    public static void main(){
        // SOQL query in apex program 
        // Simply use '[]' brackets to write query.
        // Return type of below SOQL query is List<sObject>
        // By default for every SOQL query, ID field is returned for every record. Not shown in Query editor but it's returned in Apex program
        List<Account> accounts = [select Name from Account];
        Integer count = 1;
        for(Account i :accounts){
            System.debug('No '+ count + i.Name + i.ID);
            count+=1;
        }
        
        /*
         * Return type of SOQL queries in Apex  
         * 1. List<sObject> - fetching multiple records
         * 2. sObject - fetching single record
         * 3. Integer - when we use aggregate functions
         * 4. AggregateResult - when we use aggregate functions
        */
        
        // Return type of SOQL query is sObject
        Account a1 = [select Name from Account  LIMIT 1];
        System.debug('Account is ' + a1.Name + a1.ID);
        
        // Return type Integer
        Integer account_count = [select count() from Account ];
        System.debug('Total Accounts are ' + account_count);
        
        // Return type AggregateResult 
        // Result will be map like this - {expr0=13, expr1=University of Arizona}
        AggregateResult account_count1 = [select count_distinct(Name), max(Name) from Account ];
        System.debug('Total Accounts are ' + account_count1);
        
        // Aggregate functions - 
        // 1. SUM() - Return AggregateResult
        // 2. MAX() - Return AggregateResult
        // 3. MIN() - Return AggregateResult
        // 4. COUNT() - Return Integer | Count null values also
        // 5. COUNT(field_name) - Return AggregateResult | Do not count null values
        // 6. COUNT_DISTINCT(field_name) - Return AggregateResult | Do not count null values | Count unique/distinct values only
        
		// FOR UPDATE 
        // FOR UPDATE keyword locks the queried records from being updated by any other Apex Code or Transaction. 
        // When the records are locked, other User cannot update them. Only we can UPDATE those record at that time.
       
        List<Student__c> forUpdate = [SELECT Name  FROM Student__c   FOR UPDATE];
		System.debug('For Update clause: records selected for update are '+forUpdate.size());

        // ALL ROWS - To fetch deleted records, by default SOQL query not fetch deleted records
        // If we want to use idDeleted field in Apex logic we have query it using SOQL query.
        
        // 1. This will return all records including deleted ones
        List<Account> all_rows = [select Name,isDeleted from Account ALL ROWS ];
        count = 1;
        for(Account i :all_rows){
            if(i.IsDeleted){
                System.debug('This record is deleted->');
            }
            System.debug('No '+ count + i.Name + i.ID);
            count+=1; 
        }
        
        // 2. This will return deleted records only | isDeleted field
        List<Account> deleted_rows = [select Name,isDeleted from Account where isDeleted = true ALL ROWS ];
        count = 1;
        for(Account i :deleted_rows){
            System.debug('No '+ count + i.Name + i.ID);
            count+=1; 
        }
        
        // 3. This will not return any record, because ALL ROWS not used here | isDeleted field
        // No record have isDeleted field true as ALL ROWS not used. We can deleted rows only by ALL ROWS keyword.
        List<Account> deleted_rows_doubt = [select Name,isDeleted from Account where isDeleted = true ];
        count = 1;
        for(Account i :deleted_rows_doubt){
            System.debug('No '+ count + i.Name + i.ID);
            count+=1; 
        }
	}

}
```

### Dynamic SOQL -

All the above queries are Static SOQL queries as they are written before runtime. 
Dynamic SOQL queries are written at runtime. 

```java
public class DynamicSOQL {
    
    public static void main(){
        // Dynamic SOQL means creation of SOQL string at runtime with Apex code. 
        // It is basically used to create more flexible queries based on user’s input.
        
        // Use of bind variable - `:variable`
        String name = 'Sanket';
        System.debug([select Name from Student__c where name = :name]);
        
        // Use Database.query() to create dynamic SOQL
        
        // These parameters can be taken as input from user, we delacre them explicitly here.
        String object_name = 'Student__c'; 
        String query_field = 'Name';
        System.debug(Database.query('select '+query_field+ ' from ' +object_name));

        
        // This line will give error as '' have to be there in where clause for String comparison
        // System.debug(Database.query('select '+query_field+ ' from ' +object_name+ ' where '+query_field+ '= ' + name ));
        // One way is use escape sequence.
         System.debug(Database.query('select '+query_field+ ' from ' +object_name+ ' where '+query_field+ '= ' + '\''+ name +'\'' ));
        
        // Another way is use String function - 
        // Use string escape singleQuotes(String str) on the string used for creating the query on dynamic SOQL, 
        // just to prevent SOQL injection.
        
        // 1. Above query without escape sequence and with using binding variable. For better understanding refer second query.
        System.debug(Database.query(String.escapeSingleQuotes('select '+query_field+ ' from ' +object_name+ ' where '+query_field+ '= ' +':name')));

        // 2. using binding variable as it is 
		String finalString = String.escapeSingleQuotes('select Name from Student__c where name = :name');
        System.debug(Database.query(finalString));        
        
    }
}
```

### <a id="point_number_14">Field to field comparison in an SOQL WHERE clause:point_down:</a>

[Reference](https://help.salesforce.com/s/articleView?id=000386076&type=1)

Salesforce does not allow direct field to field comparison in SOQL query. Below query will give error.

```SQL 
SELECT Id,name FROM User WHERE FirstName != Lastname
```
So for the above query, you could create a formula field on User object with return type Text e.g. **NameCompare**, with the formula

```SQL
IF(User.FirstName != User.LastName, 'true', 'false')
```

Now the query will be:

```SQL
SELECT id, name FROM User where NameCompare = 'true'
```

