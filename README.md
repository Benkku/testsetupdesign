# Test setup and teardown design
About designing good test automation cases, setups and teardowns in context of Robot Framework. Extending this article: https://github.com/robotframework/HowToWriteGoodTestCases/blob/master/HowToWriteGoodTestCases.rst

## Good functional test case

- tests only one thing that is said in test case name
- tests should be independent. Initialization using setup/teardown
- Sometimes dependencies between tests cannot be avoided.
  - For example, it can take too much time to initialize all tests separately.
  - Never have long chains of dependent tests.
  - Consider verifying the status of the previous test using the built-in
    `${PREV TEST STATUS}` variable.

## Test setup = precondition
