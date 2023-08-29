


> Written with [StackEdit](https://stackedit.io/).

**Apex primitive data types include:**

>	Blob
	Boolean - true false
	[Date](https://developer.salesforce.com/docs/atlas.en-us.244.0.apexref.meta/apexref/apex_methods_system_date.htm) 
	Datetime
	Decimal
	Double
	ID 
	Integer
	Long - specify L/l at the end
	Object 
	[String](https://developer.salesforce.com/docs/atlas.en-us.244.0.apexref.meta/apexref/apex_methods_system_string.htm) - ''(single) quotes only no ""(double) quotes
	
***`variables.apxc `***

```java
public class Variable {
    public static void main(){
        // To print line on debug - 
        System.debug('Debug'+'\n');
        System.debug(''); // empty line and '' - empty string. 
        
        // System.debug(); // gives error
        
        // Single line comment
        /*
         * Multiline comment 
         * 
        */ 
        
        Integer a = 1234;
        System.debug('Integer a ='+a +'\n');
        
        Boolean b = true; // true or false
        System.debug('Boolean b ='+b +'\n');
        
        Double d = 12.21412;
        System.debug('Double d ='+d +'\n');
        
        String s = 'Hii, welcome to salesforce';
        //String s = "Hii"; // gives error, no double quotes
        
        System.debug('String s ='+s +'\n');
        
        // Other varibles - Date ,Datetime, Decimal, ID ,Long - specify L/l at the end, Object  
    }
}
```


 **Null Variables and Initial Values** 
 
> If you declare a variable and don't initialize it with a value, it will be `null`. In essence, null means the absence of a value. You can also assign null to any variable declared with a primitive type

**Variable Scope** 

>Variables can be defined at any point in a block, and take on scope from that point forward. Sub-blocks can’t redefine a variable name that has already been used in a parent block, but parallel blocks can reuse a variable name. 
For example: 

```java
Integer i; { // Integer i; This declaration is not allowed } 

for (Integer j = 0; j < 10; j++); 
for (Integer j = 0; j < 10; j++);
```

**Constants**

 >Apex constants are variables whose values don’t change after being initialized once. Constants can be defined using the final keyword. The final keyword means that the variable can be assigned at most once, either in the declaration itself, or with a static initializer method if the constant is defined in a class.

```java
static final Integer PRIVATE_INT_CONST = 200;
```

>A non-null `String` or `ID` value is always greater than a null value

### Get the type (data type) of a field in apex

[Link](https://sfdcian.com/get-the-type-data-type-of-a-field-in-apex/)

1. #### Using Describe Method:

**Consideration Notes:**

-   It consumes a lot of CPU time.
-   There is no governor limit on this.

```java
String objectName = 'Account';
String fieldName = 'Name';

SObjectType r = ((SObject)(Type.forName('Schema.'+objectName).newInstance())).getSObjectType();
DescribeSObjectResult d = r.getDescribe();
System.debug(d.fields
        .getMap()
        .get(fieldName)
        .getDescribe()
        .getType());
```

2. #### Just a query – After Spring 20

**Consideration Notes**

-   Very less CPU time as it comes under SOQL which is not counted in CPU time calculation
-   Only 200 records can be queried.

```java
public String getFieldDataType(String objectName, String fieldName){
            return [SELECT DataType FROM FieldDefinition WHERE EntityDefinitionId=:objectName AND QualifiedApiName=:fieldName LIMIT 1].DataType;
        }
```


