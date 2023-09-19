# Test case, setup and teardown design
About designing good test automation cases, setups and teardowns in context of Robot Framework. Extending this article: https://github.com/robotframework/HowToWriteGoodTestCases/blob/master/HowToWriteGoodTestCases.rst

## Good functional test case (in Robot Framework)

- tests only one thing that is said in test case name
- tests should be independent. Initialization using setup/teardown
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

With these simple steps we can form logically two end-to-end test cases: <b> Pick And Delete Item In Basket </b> and <b> Pick And Checkout Item </b>

Test setup = precondition
