## PROFIT SPLIT PAYMENT SMART CONTRACT

Ethereum based Profit splitter Smart Contract for  Associate-level employees profit sharing payments with solidity 

![contract](screenshots/Ethereum.jpg)

In this Homewrok I will use my knowledge in Solidity to write a Smart Contract that will distribute profit/commission evenly. In this scenario I am using 3 associate level employees. In this scenario We are assuming that the profit/commission is evenly distributed between the 3 Associates; The smart contract will then evenly distribute the single deposit made to the recepients. In this activity I am deploying the Smart Contract on a private ganache network (testing purpose) in order to pay the associates from a deposit of 40 ether to be split evenly. 

### Level One: The `AssociateProfitSplitter` Contract

At the top of your contract, you will need to define the following `public` variables, Do use your own addresses for your perusal:

* `employee_one` -- The `address` of the first employee. Make sure to set this to `payable`. Address used 0x3a94020Bd2aF20b145ebFC49898c83b10AFf76D5

* `employee_two` -- Another `address payable` that represents the second employee. Address used 0x70fA164696EEE2650f6125584E530Fc4d4C104f7

* `employee_three` -- The third `address payable` that represents the third employee. Address used 
0x322b2C39CD5074B2bCFB311d7673d3bd629B076F

Create a constructor function that accepts:

* `address payable _one`

* `address payable _two`

* `address payable _three`

Within the constructor, set the employee addresses to equal the parameter values. This will allow you to avoid hardcoding the employee addresses.

Next, create the following functions:

* `balance` -- This function should be set to `public view returns(uint)`, and must return the contract's current balance. Since we should always be sending Ether to the beneficiaries, this function should always return `0`. If it does not, the `deposit` function is not handling the remainders properly and should be fixed. This will serve as a test function of sorts.

* `deposit` -- This function should set to `public payable` check, ensuring that only the owner can call the function.

  * In this function, perform the following steps:

    * Set a `uint amount` to equal `msg.value / 3;` in order to calculate the split value of the Ether.

    * Transfer the `amount` to `employee_one`.

    * Repeat the steps for `employee_two` and `employee_three`.

    * Since `uint` only contains positive whole numbers, and Solidity does not fully support float/decimals, we must deal with a potential remainder at the end of this function since `amount` will discard the remainder during division.

    * We may either have `1` or `2` wei leftover, so transfer the `msg.value - amount * 3` back to `msg.sender`. This will re-multiply the `amount` by 3, then subtract it from the `msg.value` to account for any leftover wei, and send it back to Human Resources.

* Create a fallback function using `function() external payable`, and call the `deposit` function from within it. This will ensure that the logic in `deposit` executes if Ether is sent directly to the contract. This is important to prevent Ether from being locked in the contract since we don't have a `withdraw` function in this use-case.

#### Test the contract

In the `Deploy` tab in Remix, deploy the contract to your local Ganache chain by connecting to `Injected Web3` and ensuring MetaMask is pointed to `localhost:8545`. Also ensure that you have 4 accounts ready to for the contract.
![contract](screenshots/RemixWeb3.PNG)
![contract](screenshots/MetaMaskLocal.PNG)
![contract](screenshots/ganache1.PNG)


Once deployed, metamask prompt will allow to verify the transaction and fees; considering that you have used an account with funds, upond confirmation of transaction in your remix bottom pane (I think it's the Activity Log) a green check mark will appear to show that the action was successful.

![contract](screenshots/DeployedCheck.PNG)

You will need to fill in the constructor parameters with your designated `employee` addresses. 

![contract](screenshots/SolidityDeposit.PNG)

Test the `deposit` function by sending various values. Keep an eye on the `employee` balances as you send different amounts of Ether to the contract and ensure the logic is executing properly. Sending 40 ether to the 3 associate employee accounts. 

![contract](screenshots/Deployed.PNG)
![contract](screenshots/MetaMaskTransact.PNG)

As you can see below we have subtracted 40 ether from the original account and split it between the 3 designated accounts. 
![contract](screenshots/ganache2.PNG)