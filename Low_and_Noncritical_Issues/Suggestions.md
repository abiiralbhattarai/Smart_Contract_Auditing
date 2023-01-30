# Important Suggestion

| Number | Issues                                                                                                  |
| :----: | :------------------------------------------------------------------------------------------------------ |
|   1.   | [Generate perfect code headers every time](#generate-perfect-code-headers-every-time)                   |
|   2.   | [Make the Test Context with Solidity](#make-the-test-context-with-solidity)                             |
|   3.   | [Project Upgrade and Stop Scenario should be there](#project-upgrade-and-stop-scenario-should-be-there) |

---

1.  ## Generate perfect code headers every time

    Description: I recommend using header for Solidity code layout and readability

    https://github.com/transmissions11/headers

    ```
    /*//////////////////////////////////////////////////////////////
                         TESTING 123
    //////////////////////////////////////////////////////////////*/

    ```

2.  ## Make the Test Context with Solidity

    - It's crucial to write tests with possibly 100% coverage for smart contract systems.

    - It is recommended to write appropriate tests for all possible code streams and especially for extreme cases.

    - But the other important point is the test context, tests written with solidity are safer, it is recommended to focus on tests with Foundry

3.  ## Project Upgrade and Stop Scenario should be there

    - At the start of the project, the system may need to be stopped or upgraded, I suggest you have a script beforehand and add it to the documentation. This can also be called an " EMERGENCY STOP (CIRCUIT BREAKER) PATTERN ".

    - https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol

---
