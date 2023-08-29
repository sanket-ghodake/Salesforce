


> Written with [StackEdit](https://stackedit.io/).

## Salesforce Object Search Language

Used to perform text based search on all of the reocords in database irrespective of objects.

While creating Custom objects you should click :ballot_box_with_check: ` Allow Search` field. 
If the field is :black_square_button: untick then SOSL will not work on that object.
 
Numeric fields are not searchable.

Text searches are case-insensitive.

Search results are strings or can be substrings. 
(Find {Sanket} - can match name 'Sanket' or 'abcdSanketabcd') 

In query editor `{ }` and in Apex `' '`

```SQL 
// SOSL Syntax 
// In Apex 
Find 'What' In [What Fields] Returning [Where Objects]

// In Query Editor 
Find {What} In [What Fields] Returning [Where Objects]

Find{Sanket}
// give all reocrds with Sanket in there field name from all objects
// Output will have Object names as Columns
// Above query will not work in Apex as we have to mentioned object names in Returning clause
// By default query retunrn ID's of objects found. 

// IN ALL Fields - search in all fields
Find {Sanket} IN ALL fields 
Find {Sanket} IN Name fields

// Search groups - Use in `IN` clause
// ALL FILEDS - by default - search across all String fields
// NAME FILEDS
// EMAIL FILEDS
// PHONE FILEDS
// SIDEBAR FILEDS - To search into `Sidebar` present in Classic UI 

// RETURNING - we can give particular objects for search 
Find {Sanket} IN ALL fields Returning Account, Student__c

// Return particular fileds - by sepcifying them in bracked after object name
Find {Sanket} IN ALL fields Returning Account, Student__c (Name, ID)

//If you want to specify a WHERE, ORDER BY, LIMIT clause, you must include a field list with at least one specific field.

// WHERE - in each object we can give filter condition after field name
Find {Sanket} IN ALL fields Returning Account, Student__c (Name where CreatedDate < Today)

// ORDER BY - sort results
Find {Sanket} IN ALL fields Returning Account, Student__c (Name order by Name)

// LIMIT - Limit results
// Limit should always be the last statement.
Find {Sanket} IN ALL fields Returning Account, Student__c (Name LIMIT 50)

// Wildcards 
// 1. * - 0 or N number of characters
Find {San*}
// 2. ? - Singl character
Find {San?et}

```

### SOSL in Apex and Dynamic SOSL

```java
public class SOSL {
    public static void main(){
        // SOSL Syntax 
        // In Apex 
        //Find 'What' In [What Fields] Returning [Where]
        
        //Find 'Sanket' - will give error in Apex. 
        //We have to mentioned objects for which search to be perform
        System.debug(Search.query('Find \'Sanket\'Returning Account'));
        
        // SOSL query will return List of List of sObjects
        // A SOSL query returns a list of list of sObjects and it can be performed on multiple objects.
        // First dimension will be Every object name that matches search, and second dimension will have all search records from that objects.
        List<List<Account>> search_query = [Find 'Sanket'Returning Account];
        System.debug(search_query);
        
        // Search from multiple objects
        search_query = [Find 'Sanket' Returning Account, Student__c];
        System.debug(search_query);
        
        // By default query return only ID field for searched objects. 
         
        
        // Dynamic SOSL - 
        // Search.query(query at runtime) - Use this function for Dynamic SOSL queries
        
        // Using escape sequence
        System.debug(Search.query('Find \'Sanket\'Returning Account'));
        
        // Using String.escapeSingleQuotes
        String search_pattern = 'Sanket';
        System.debug(Search.query(String.escapeSingleQuotes('Find :search_pattern Returning Account')));
    }
}
```
