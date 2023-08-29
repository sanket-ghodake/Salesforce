


> Written with [StackEdit](https://stackedit.io/).

# [Using Limits,  startTest, and  stopTest](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_testing_tools_start_stop_test.htm?_ga=2.161160829.1755463636.1692668258-1842203621.1692668258)

The test method contains the `Test.startTest()` and `Test.stopTest()` method pair, which delimits a block of code that gets a fresh set of governor limits. In this test, test-data setup uses two DML statements before the test is performed. To test that Apex code runs within governor limits, isolate data setup’s limit usage from your test’s. To isolate the data setup process’s limit usage, enclose the test call within the `Test.startTest()` and `Test.stopTest()` block. Also use this test block when testing asynchronous Apex.


Each method has two versions. The first version returns the amount of the resource that has been used in the current context. The second version contains the word “limit” and returns the total amount of the resource that is available for that context. For example, `getCallouts` returns the number of callouts to an external service that have already been processed in the current context, while `getLimitCallouts` returns the total number of callouts available in the given context.

In addition to the Limits methods, use the `startTest` and `stopTest` methods to validate how close the code is to reaching governor limits.

The `startTest` method marks the point in your test code when your test actually begins. Each test method is allowed to call this method only once. All of the code before this method should be used to initialize variables, populate data structures, and so on, allowing you to set up everything you need to run your test. Any code that executes after the call to `startTest` and before `stopTest` is assigned a new set of governor limits.
