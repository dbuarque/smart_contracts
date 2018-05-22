# Hello, World

This example is about deploying a simple samrt contract on Travis Testnet.

###### The source code 'HelloWorld.sol' is also in the directory.

The contract has two main methods: sayHello() and updateMessage(). 

sayHello() returns a greeting message *(type bytes32)* to its caller which is initially set to 'Hello, World' when the Smart Contract is deployed.  

updateMessage() allows the method caller to change the greeting message to another message passed by function argument *(type bytes32)*.  

## Solidity

One option for getting the key information is to compile the .sol file in your command console using Solidity compiler.

See installation instructions here: <http://solidity.readthedocs.io/en/v0.4.21/installing-solidity.html>

Once you have *Solidity* installed, go create the smart contract file **HelloWorld.sol** (or download/pull/copy directly from this directory) and run:

  *solc --abi HelloWorld.sol*
  
  *solc --bin HelloWorld.sol*
  
  *solc --gas HelloWorld.sol*
  
for obtaining the associated ABI definition, compiled EVM bytecode and gas fee estimation of the contract.

## Remix IDE

Another option is to compile it from a user-friendly IDE.

Access the IDE here: <http://remix.ethereum.org>

The IDE compiles and creates the ABI, bytecode, gas fee estimation in a convenient manner. The information can be obtained ubder the **compile/details** section, indicated as **'ABI'**, **'BYTECODE'** and **'GASESTIMATES'**. Other info under the section as an auxiliary.

## Deployment on Travis 

Now since we have the key information, this contract is ready to be deployed from GETH or Travis and obtain an address on the blockchain for the deployed contract instance. 

The following commands can be run in the GETH or Travis console attached to the *Travis network*.

###### > abi = ...

###### > bytecode = '0x...'

###### > gas = ... *(please notice this is supposed to be a single number rather than a JSON structure)*

Then you need to have your own unlocked account with balance to deploy it.

###### > deploy = { from: "0x...", data: bytecode, gas: gas }

###### > helloContract = cmt.contract(abi)

###### > hello = helloContract.new("0x...", deploy) *(please notice the first argument is the same as the 'from' argument in 'deploy' is supposed to be simple 'address' type)

If authentication error is encountered, make sure you unlock the account before deployment.

Once the contract is mined and recorded on the blockchain, you should be able to create an instance of it and call its public methods. 

###### > hello2 = helloContract.at(hello.address)

###### > hello2.sayHello()
###### "0x"

###### > hello2.updateMessage("hi")
###### "0x..."

The sayHello() method does not change the internal state, so execution of it doesn't have cost. However, updateMessage changes the state and gas fee is required. The account to pay can be specified as a function argument or if not specified, the default account set as: 

###### > cmt.defaultAccount="0x..."

## Simplified deployment by Truffle

*Truffle* automates and simplifies the deployment process. 


