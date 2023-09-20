# Test case, setup and teardown design
About designing good test automation cases, setups and teardowns. Examples made for Robot Framework, extending this article: https://github.com/robotframework/HowToWriteGoodTestCases/blob/master/HowToWriteGoodTestCases.rst

## Good functional test case

- tests only one thing that is said in test case name
- tests should be independent (Also terms idempotent and isolated are used). Initialization using setup/teardown
- Sometimes dependencies between tests cannot be avoided.
  - For example, it can take too much time to initialize all tests separately.
  - Never have long chains of dependent tests.
  - Consider verifying the status of the previous test using the built-in
    `${PREV TEST STATUS}` variable.

## Webshop example
<table style="width:100%">
  <tr>
    <th>Feature</th>
    <th>Test Step</th>
    <th>Example in webshop</th>
  </tr>
  <tr>
    <td>Feature 1</td>
    <td>Test Step 1</td>
    <td>Pick item to purchase</td>
  </tr>
  <tr>
    <td>Feature 2</td>
    <td>Test Step 2</td>
    <td>Delete item from basket</td>
  </tr>
  <tr>
    <td>Feature 3</td>
    <td>Test Step 3</td>
    <td>Checkout item</td>
  </tr>
</table>

With these simple steps we can form logically two end-to-end test cases: <b> Pick And Delete Item In Basket </b> and <b> Pick And Checkout Item </b>. Correspondingly, we mark these test cases with features: F1 -> F2 and F1 -> F3. 

Why this is not the best idea to form test cases? We should NOT test several things with one test case. Why not? Because if Feature 1 fails, we probably dont know about sanity of F2 and F3, because we dont get there. Also, we dont see immediately from test case name and status, which feature failed.

We should test all three features separately and <i>independently</i>. That means we can run each test case without others. Because test steps 2 and 3 requires step 1, it is a <i>precondition</i> as well as a feature to test. Hence, we define three test cases, where two of them has Test Step 1 as precondition, i.e. put it in Test Setup in Robot code.

Robot Framework code:

    *** Test Cases ***
    Picking Item
        Pick Item To Purchase
  
    Delete Item From Basket
        [Setup]    Pick Item To Purchase
        Delete Item From Basket

    Checkout Item In Basket
        [Setup]    Pick Item To Purchase
        Checkout Item

Now, the first test case indicates immediately if Item Picking is broken. Second and third case would then fail in setup phase, but we can notice that cases itself could not be run. That is different information from failure, which would happen if Picking Item would be inside test case. 

<i>Postcondition</i> for <b> Pick Item </b> is that there is item in basket. However, the other two test cases may not leave the item there. That's why we need Teardown part to empty basket. That could be implemented in a way, that it does not fail if the basket is empty already, so we can use it as suite teardown. Then it initializes the basket empty even if test cases 2 of 3 fails to do that and allows the other cases to be run. Suite setup/teardown could look like this: 

    *** Settings ***
    Suite Setup        Open Browser To Webshop
    Suite Teardown     Empty Basket

## Summary

Running this kind of test cases, we can clearly pinpoint which feature fails. After getting the fix, the corresponding test case indicates whether the function fix is working. However, if a setup part of a test case is failing, we notice from the result log that the feature corresponding the case may not be failing, but it is not run at all. 

This kind of dividing test cases in small independent units and thinking of the role of the setups and teardowns is significant when the test suites and run reports become massive. The reason for failure can be translated directly from the name of test case. There is no need to dig deeply into result report. Although long chaining of test steps can not be avoided always, the careful design of test case architecture is  sometimes seen undervalued. 
