


# AutoSale
This example is about deploying a decentralized auto sale smart contract on the blockchain.

**The source code 'AutoSale.sol' is available in the directory.**

This contract has 8 data fields (buyer, seller, price, titleId, make, model, year and color) and 1 main method (buyCar).

buyCar() sets the buyer field to address of the caller of the function and then transfers the funds from the buyer to the seller. 

## Remix IDE

To test the smart contract on the Remix IDE, go to [https://remix.ethereum.org/](https://remix.ethereum.org/). 

First, paste the solidity code for the auto sale over the voting smart contract sample code provided by remix. 

Then, **select the "Compile" tab** on the right hand portion of the window. **Click on the "Start to compile" button** to compile the code. 

After that, **select the "Run" tab** on the right hand portion of the window. Ensure that the environment is **set to "JavaScript VM".**

**Click on the "Deploy" button.** This will deploy the smart contract onto the blockchain. The first default account on the "Accounts" field should now have slightly less than 100 ether. This is because it deployed the contract and had to pay a gas fee to do so. You should now also have some accessor functions for every public variable (blue buttons) and the main function (red button) appear at the bottom right corner of the window. Click on these to call the functions.

To complete a sale, scroll back up to the "Account" field in the "Run" tab. Select any account that is not the first one. In the **"Value" text field, enter in the price for the automobile** (in this case, 9 ether or 9000000000000000000 wei). If you input a value that is not equal to the price of the automobile, the buyCar() function will not run and the sale will not complete. **Click on the red "buyCar" button** on the lower bottom right hand side of the window. The sale should now be complete.

The accounts listed under the "Accounts" button should now have updated balances to reflect the sale. The buyer variable should now also be updated to the address of the account you selected earlier to reflect it as well. To check this, **click on the blue "buyer" button** on the bottom right hand of the screen. 

Take note that the way in which this smart contract is designed, a sale can only be made once. Attempting to call the buyCar() function a second time will not succeed. 

## Travis Testnet deployment with Solidity

To deploy AutoSale on Travis Testnet with Solidity, we will need the Solidity compiler. For instructions on how to install the Solidity compiler, go [here](http://solidity.readthedocs.io/en/v0.4.21/installing-solidity.html). 

First, start up your Travis node with 

    travis node start --home=./.travis
 
 Then, open up a new terminal window and navigate to the "AutoSale.sol" file.
 
 After that, proceed to enter the following commands

    solc --abi AutoSale.sol
    solc --bin AutoSale.sol
    solc --gas AutoSale.sol

This will return the ABI, bytecode and the gas fee estimation for the AutoSale smart contract.

Attach to the Travis node with

    travis attach http://localhost:8545

and enter the following with the associated ABI, bytecode and gas fee information

    abi = (enter the output of "solc --abi AutoSale.sol" here)
    bytecode = "(enter the output of "solc --bin AutoSale.sol" here)"
    gas = (enter the integer value estimation of the JSON output from "solc --gas Autosale.sol")
Note: Make sure that you enclose your Bytecode in quotation marks or else the compiler will read your bytecode variable value as infinity.

After this, unlock your Travis account and ensure that it has enough balance to deploy the smart contract.

Unlock your Travis Account by entering

    personal.unlockAccount("0xACCOUNT_NUMBER", "PASSWORD")

Once you have unlocked your account, enter

    deploy = { from: "0xACCOUNT_NUMBER", data: bytecode, gas: gas }
    autoContract = cmt.contract(abi)
    auto = autoContract.new("0xACCOUNT_NUMBER", deploy)

Once the contract is mined and recorded on the blockchain, you will be able to interact with it.

To interact with the contract, enter

    auto2 = autoContract.at(auto.address)

This will create an instance of the contract. Once we have this, we will be able to call the public methods of the contract. For example

    auto2.make()
    auto2.model()
    auto2.year()
    
More instructions on interacting with smart contracts to be added soon!

## Travis Testnet deployment with Truffle

To deploy AutoSale on Travis Testnet, we will use Truffle. To install Truffle, go [here](http://truffleframework.com/docs/getting_started/installation). To learn how to run your own Travis node, go [here](https://medium.com/cybermiles/running-a-travis-node-ac7447b754d4).

First, open up your terminal and enter 

`source $HOME/.gvm/scripts/gvm`

Then, start up your travis node with 

`travis node start --home=./.travis`

Open up a new terminal window and create a new folder for the AutoSale Smart Contract anywhere you like. I will refer to this folder as the "AutoSale" folder.

Navigate to that folder in the terminal and enter 

`truffle init`

This will auto-generate template files for you within said folder. 

Now, navigate to your AutoSale folder and within it should reside another folder named "contracts". In the "contracts" folder there should be a file named "Migrations.sol". **Do not** mess with this file.

Create a new Solidity file in the "contracts" folder named "AutoSale.sol". Paste the source code into this file. 

Navigate back to your AutoSale folder and find the directory named "migrations". Go into this directory and you should see a file named "1_initial_migration.js". Again, **do not** mess with this file and instead create a new file called "2_deploy_contracts.js". In this newly created file, paste the code below. 

    var AutoSale = artifacts.require("./AutoSale.sol");
    module.exports = function(deployer) {
      deployer.deploy(AutoSale);
    };

Navigate back to your AutoSale folder and open the "truffle.js" file. Paste the code below into this file.

    module.exports = {
      networks: {
        development: {
          host: "127.0.0.1",
          port: 7545,
          network_id: "*" // Match any network id
        },
        travis: {
          host: "localhost",
          port: 8545,
          network_id: "*",
          from: "0xFROM_ADDRESS",
          gas: -,
          gasPrice: -
        }
      }
    };

Make sure you replace "0xFROM_ADDRESS" with an actual address you created on the Travis Testnet and fill in the desired gas and gasPrice.

Go back to your terminal window and enter

    truffle compile

Open a new terminal window and enter

    source $HOME/.gvm/scripts/gvm
    travis attach http://localhost:8545
    > personal.unlockAccount("0xFROM_ADDRESS","PASSWORD")

This will unlock your account and allow you to deploy the smart contract with the account. 

Go back to the previous terminal window and enter

    truffle migrate --network travis

Congrats! You have now deployed a smart contract on the Travis Testnet!

More instructions and updates of how to deploy smart contracts on Travis Testnet, TestRPC and ETH testnet to follow soon.