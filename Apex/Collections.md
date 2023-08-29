


> Written with [StackEdit](https://stackedit.io/).

### Lists 

> A list is an ordered collection of elements that are distinguished by their indices. List elements can be of any data type—primitive types, collections, sObjects, user-defined types, and built-in Apex types.
 
> The index position of the first element in a list is always 0. Lists can contain any collection and can be nested within one another and become multidimensional. For example, you can have a list of lists of sets of Integers.

[List class methods](https://developer.salesforce.com/docs/atlas.en-us.244.0.apexref.meta/apexref/apex_methods_system_list.htm)


### Sets 

>A set is an unordered collection of elements that do not contain any duplicates. Set elements can be of any data type—primitive types, collections, sObjects, user-defined types, and built-in Apex types.

[Set class methods](https://developer.salesforce.com/docs/atlas.en-us.244.0.apexref.meta/apexref/apex_methods_system_set.htm)

### Maps 

>A map is a collection of key-value pairs where each unique key maps to a single value. Keys and values can be any data type—primitive types, collections, sObjects, user-defined types, and built-in Apex types.

[Maps class methods](https://developer.salesforce.com/docs/atlas.en-us.244.0.apexref.meta/apexref/apex_methods_system_map.htm)

- A map key can hold the `null` value. 
- Adding a map entry with a key that matches an existing key in the map overwrites the existing entry with that key with the new entry
- Map keys of type String are case-sensitive. Two keys that differ only by the case are considered unique and have corresponding distinct Map entries.


***`collection.apxc`***
```java 
public class Collection {
    public static void main(){
        // Collection - 
        
        // 1.
        // List - sequential order and can be duplicate, indexing start with 0
        // List<data_type> name = new List<data_type>{1,2,3};
        // List<data_type> name = new List<data_type>();
        
        // Create an empty list of String
        List<String> my_list_1 = new List<String>();
        System.debug('List is -'+ my_list_1);
        // Create a nested list
        List<List<Set<Integer>>> my_list_2 = new List<List<Set<Integer>>>();
        System.debug('Nested List is -'+ my_list_2);
        
        // add() - add element
        // get(index) - get value of particular index
        
        my_list_1.add('First element');
        System.debug('List is -'+ my_list_1);
        
        System.debug('Get first element -'+ my_list_1.get(0));
        
        // List using array notation 
        String[] colors = new String[1];
        System.debug('colors list is -'+ colors);
        
        // List and ListArray both are dynamic in size, i.e they are elastic.
        // While in ListArray minimum array size is defined when we initialize it.
        
		//2.
		//Sets - not in sequential order, unique values
		
        Set<String> myStringSet = new Set<String>();
        System.debug('Set is -'+ myStringSet);
        
        // Defines a new set with two elements
		Set<String> set1 = new Set<String>{'Aakash', 'Rohit'};
        System.debug('Second Set is -'+ set1);
		
        // add(), contains(element)
        
        set1.add('Aakash'); // adding element to set
        System.debug('Second Set is -'+ set1);
        
        System.debug('Is Rohit is in Set -'+ set1.contains('Rohit'));
        
        //3.
        //Maps - not in sequential order, key-value pair, key is unique, value can be duplicate
        
        Map<String, String> country_currencies = new Map<String, String>(); // value is String type
        System.debug('Map is -'+ country_currencies);
		Map<ID, Set<String>> m = new Map<ID, Set<String>>(); // value is Set type
        System.debug('Map is -'+ m);
        
        Map<Integer, String> students = new Map<Integer, String>{1 =>'Aakash', 2=>'Rohit'};
        System.debug('Map is -'+ students);
        
        //put(key,value) - insert key value pair
        students.put(3,'Vedant');
        System.debug('Map is -'+ students);

        //get(key_name) - to access value
        System.debug('Value of 2 student in Map is -'+ students.get(2));
        
        // Keys - only be primitive data types
        // Values - can be primitive, sObjects, Apex objects and other collections 
    }
}
```
