


> Written with [StackEdit](https://stackedit.io/).

## Child to parent

```SQL
select Name, Account.Name from Contact
```
Here `Account` is relationship field name.

```SQL 
select Name, Student__r.Name from Courses
```
Here `Student__r` is relationship field name appended with `__r` for Custom relationship field

To access this fields in code we have to write same thing,
example - To get Studen name related to course - 
` Student__r.Name`

## Parent to child

```SQL
select Name,(select LastName, Phone from Contacts) from Account
```
`Contacts`  is Child relationship name, it is given under relationship field details.

`(select LastName, Phone from Contacts)` is a sub-query.

```SQL
selct  Name, StudentEmail__c, (select Name, Rating__c from Ratings__r) from Student__c
```

`Ratings` is Child relationship name, appended with `__r` as this is custom relationship field. 

`(select Name, Rating__c from Ratings__r)` is a sub-query.

---

When to use which queries -> 

As every parent have multiple childs we can get all childrens by writing sub-query.

Every child will have exactly one parent, so we can get Parent fields by `.` dot operator on Child.

---

## Multilevel Relationship Queries

OpportunityLineItem -> Opportunity -> Account
Student -> Courses -> Faculties

### Parent to child

A sub-query can not have query inside it.
So we can't traverse from Parent to child for getting multilevel relations

### Child to parent

In SOQL, you can traverse up to a maximum of five levels when querying data from child object to parent.

```SQL
SELECT Name, Position__r.Contact.Account.Owner.FirstName FROM Job_Application__c;
```

## Query for many to many relationship -

```SQL
select Course__r.Name, count(Student__r.Name)
from StudentCourses__c
group by Course__r.Name
```

Here `StudentCourses__c` is junction object and `Course__r`, `Student__r` are master-detail relationship fields on Course and Student object respectively.

