# Gearing Up

This is the walkthrough of the second lab from casinoheist.xyz. It start with creating a new Solidility Project using Foundry-Forge

```bash
$ forge init gearingup
Initializing /mnt/c/Users/hzqzz/Downloads/casinoheist/gearingup...
Installing forge-std in /mnt/c/Users/hzqzz/Downloads/casinoheist/gearingup/lib/forge-std (url: Some("https://github.com/foundry-rs/forge-std"), tag: None)
Cloning into '/mnt/c/Users/hzqzz/Downloads/casinoheist/gearingup/lib/forge-std'...
remote: Enumerating objects: 2096, done.
remote: Counting objects: 100% (1027/1027), done.
remote: Compressing objects: 100% (141/141), done.
remote: Total 2096 (delta 943), reused 898 (delta 886), pack-reused 1069 (from 1)
Receiving objects: 100% (2096/2096), 676.90 KiB | 2.04 MiB/s, done.
Resolving deltas: 100% (1419/1419), done.
    Installed forge-std v1.9.6
    Initialized forge project
```

```bash
$ tree
.
└── gearingup
    ├── foundry.toml
    ├── lib
    │   └── forge-std
    │       ├── CONTRIBUTING.md
    │       ├── foundry.toml
    │       ├── LICENSE-APACHE
    │       ├── LICENSE-MIT
    │       ├── package.json
    │       ├── README.md
    │       ├── scripts
    │       │   └── vm.py
    │       ├── src
    │       │   ├── Base.sol
    │       │   ├── console2.sol
    │       │   ├── console.sol
    │       │   ├── interfaces
    │       │   │   ├── IERC1155.sol
    │       │   │   ├── IERC165.sol
    │       │   │   ├── IERC20.sol
    │       │   │   ├── IERC4626.sol
    │       │   │   ├── IERC721.sol
    │       │   │   └── IMulticall3.sol
    │       │   ├── safeconsole.sol
    │       │   ├── Script.sol
    │       │   ├── StdAssertions.sol
    │       │   ├── StdChains.sol
    │       │   ├── StdCheats.sol
    │       │   ├── StdError.sol
    │       │   ├── StdInvariant.sol
    │       │   ├── StdJson.sol
    │       │   ├── StdMath.sol
    │       │   ├── StdStorage.sol
    │       │   ├── StdStyle.sol
    │       │   ├── StdToml.sol
    │       │   ├── StdUtils.sol
    │       │   ├── Test.sol
    │       │   └── Vm.sol
    │       └── test
    │           ├── compilation
    │           │   ├── CompilationScriptBase.sol
    │           │   ├── CompilationScript.sol
    │           │   ├── CompilationTestBase.sol
    │           │   └── CompilationTest.sol
    │           ├── fixtures
    │           │   ├── broadcast.log.json
    │           │   ├── test.json
    │           │   └── test.toml
    │           ├── StdAssertions.t.sol
    │           ├── StdChains.t.sol
    │           ├── StdCheats.t.sol
    │           ├── StdError.t.sol
    │           ├── StdJson.t.sol
    │           ├── StdMath.t.sol
    │           ├── StdStorage.t.sol
    │           ├── StdStyle.t.sol
    │           ├── StdToml.t.sol
    │           ├── StdUtils.t.sol
    │           └── Vm.t.sol
    ├── README.md
    ├── script
    │   └── Counter.s.sol
    ├── src
    │   └── Counter.sol
    └── test
        └── Counter.t.sol

13 directories, 54 files
```

The important one is in script, src and test folder. So lets remove the files in the folder mentioned.

```bash
$ rm script/Counter.s.sol src/Counter.sol test/Counter.t.sol
```

Now the project is ready. First download the challenge files. We got GearingUp.sol and Setup.sol. Move it into the src folder. Then create Exploit.sol because we will use solve script to solve this challenge.

```bash
┌──(zeqzoq㉿zeqzoq)-[/mnt/c/Users/hzqzz/Downloads/casinoheist/gearingup/src]
└─$ ls
Exploit.sol  GearingUp.sol  Setup.sol
```

Now we are writing the prerequisite of exploit script. I'll attach the explanation in the code. But remind me to go through this code again because Im not confident enough to explain about this right now.

{% code title="Exploit.sol" lineNumbers="true" %}
```solidity
pragma solidity ^0.8.26; //declaring the Solidility version

import "./Setup.sol"; //import the challenge files first
import "./GearingUp.sol";

contract Exploit{ //this will create a variable that points to this contract instance.
//the name is define after the word contract, so it make sense we are naming this as Exploit for Exlpoit.sol file.
    Setup public setup;            // this  is basically initialization of the files. You can do like this
    GearingUp public immutable GU; // or this like this

    constructor(address _setup) { //now to assign the address
        setup = Setup(_setup); //this is to assign Setup address to variable setup
        GU = GearingUp(setup.GU()); //or you can assign like this. This one is just assigning GearingUp address by using setup
        //because GU is initialized in setup.sol. So technically we are using that.
    }
}
```
{% endcode %}

So thats all for the project setup using forge. Now we are solving this challenge. Lets start with reviewing the code.

{% code title="Setup.sol" lineNumbers="true" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
import "./GearingUp.sol";

contract Setup {
    GearingUp public GU;

    constructor() payable{
        GU = new GearingUp{value: 10 ether}();
    }

    function isSolved() public view returns(bool){
        return GU.allFinished();
    }
}
```
{% endcode %}

{% code title="GearingUp.sol" lineNumbers="true" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
contract GearingUp{

    bool public callOne;
    bool public depositOne;
    bool public withdrawOne;
    bool public sendData;
    bool public allFinished;

    constructor() payable{
        require(msg.value == 10 ether);
    }

    function callThis() public{
        // verify that a smart contract is calling this.
        require(msg.sender != tx.origin);
        callOne = true;
    }

    function sendMoneyHere() public payable{
        require(msg.sender != tx.origin);
        require(msg.value == 5 ether);
        depositOne = true;
    }

    function withdrawReward() public{
        require(msg.sender != tx.origin);
        (bool transfered, ) = msg.sender.call{value: 5 ether}("");
        require(transfered, "Failed to Send Reward!");
        withdrawOne = true;
    }

    function sendSomeData(string memory password, uint256 code, bytes4 fourBytes, address sender) public{
        if(
            keccak256(abi.encodePacked(password)) == keccak256(abi.encodePacked("GearNumber1")) &&
            code == 687221 &&
            keccak256(abi.encodePacked(fourBytes)) == keccak256(abi.encodePacked(bytes4(0x1a2b3c4d))) &&
            sender == msg.sender
        ){
            sendData = true;
        }
    }

    function completedGearingUp() public{
        if(
            callOne == true &&
            depositOne == true &&
            withdrawOne == true &&
            sendData == true
        ){
            allFinished = true;
        }
    }
}
```
{% endcode %}

So what do we need to make `Setup::isSolved()` return `True`?

* `GearingUp::callOne()` must be `True`
* `GearingUp::depositOne()` must be `True`
* `GearingUp::withdrawOne()` must be `True`
* `GearingUp::sendData()` must be `True`
* `GearingUp::allFinished()` must be `True`

We see that the contructor in GearingUp said that it need 10 Ether to be deployed. To solve this challenge, instead of we enter the command in the terminal like we did previously, we will be doing it in the exploit script. So I'll update the exploit script step by step. First we want to make `GearingUp::callOne()` to return `true`.

```solidity
function callThis() public{
        // verify that a smart contract is calling this.
        require(msg.sender != tx.origin);
        callOne = true;
}
```

It compares the `msg.sender` value with the `tx.origin`. We know from the `briefing` that `msg.sender` can be either EOA or Smart Contract, but tx.origin will always be an EOA, so to solve this we need to call the function using a Smart Contract, in this case our Exploit Contract. We can call the function by implementing this in our Smart Contract.&#x20;

```solidity
function solveGearingUp() public {
    GU.callThis(); // Calling function
}
```

running the `Exploit::solveGearingUp()` will call the `GearingUp::callOne()` returns true now.  Now lets make `GearingUp::depositOne()` and `GearingUp::withdrawOne()` return true. Here's the code snippet:

```solidity
function sendMoneyHere() public payable{
        require(msg.sender != tx.origin);
        require(msg.value == 5 ether);
        depositOne = true;
}
```

```solidity
function withdrawReward() public{
        require(msg.sender != tx.origin);
        (bool transfered, ) = msg.sender.call{value: 5 ether}("");
        require(transfered, "Failed to Send Reward!");
        withdrawOne = true;
}
```

We will exploit both simultaneously because there are similar, and both require 5 Ether. Heres the updated exploit:

```solidity
function solveGearingUp() public payable{
    GU.callThis();
    GU.sendMoneyHere{value: msg.value}(); // ensure that msg.value == 5
    GU.withdrawReward(); // Take Reward
}

receive() external payable{}
```

Since our contract didn't have Ether, we need to make `Exploit::solveGearingUp()` become a _payable_ function by adding the word _payable_ there. Now to make `GearingUp::sendData = true`. We just need to send "GearNumber1", 687221, 0x1a2b3c4d and sender == msg.sender. sender == msg.sender means that we need to send address. Since it compares that the value of _address sender_ is equal with _msg.sender_ (our smart contract), we must put our address there. In another case, when you need to parse a payable address, you can use _payable(address(this))_. Here's the exploit

```solidity
function solveGearingUp() public payable{
    GU.callThis();
    GU.sendMoneyHere{value: msg.value}();
    GU.withdrawReward(); 
    // Send the required data
    GU.sendSomeData("GearNumber1", 687221, 0x1a2b3c4d, address(this));
}
```

Now that all of them returned true, the last is to call `GearingUp::completedGearing()` to make the `Setup::isSolved()` return true and solve the lab. Here's the final exploit

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
import "./Setup.sol";
import "./GearingUp.sol";

contract Exploit{
    Setup public setup;
    GearingUp public immutable GU;

    constructor(address _setup){
        setup = Setup(_setup);
        GU = GearingUp(setup.GU());
    }

    function solveGearingUp() public payable{
        require(msg.value == 5 ether);
        GU.callThis();
        GU.sendMoneyHere{value: msg.value}();
        GU.sendSomeData("GearNumber1", 687221, 0x1a2b3c4d, address(this));
        GU.withdrawReward();
        GU.completedGearingUp();
    }

    receive() external payable { }
}
```

To deploy the exploit, the lab used this command

```bash
forge create src/gearing-up/Exploit.sol:Exploit -r $RPC_URL --private-key $PK --constructor-args $SETUP_ADDR 
```

Let's get our credential.

```bash
$ curl -sSfL https://pwn.red/pow | sh -s s.AAAnEA==.PCiOrFQWjZDsl2WtLRgUcQ==
s.aevSbdh9eOraZ5Rpc4XVlFax6E+LS2m+4ljcqje2MSH9S4oa638hmqOFQ5esBAj06D2q4hhigz1mKvxpVnf6XpdHo6qsOIj5z/jZrwRQouFa9kSM0MJ7typknr6vRXqxxiBCw/G237Y0UWHe5KyM9CUSVepm6SGTJadEmQXpl7JSODIuuq94ui9dnCWEChCRA3E4lGVO0IMrxlU1xsTU6w==
```

<figure><img src="../../.gitbook/assets/Screenshot (31).png" alt=""><figcaption></figcaption></figure>

```bash
$ export SETUP_ADDR=0x9954160F4F82eb263f2f81599Da2C36Ba62AA0E4
$ export RPC_URL=http://103.178.153.113:30002/2ca0ce07-e040-482c-bd97-df6c03bf7c3a
$ export PK=0xaf0fafd28c6095303239fa13a86d15ca53c6f47cd31fe50f9d8c011b077d394b
```

```bash
$ forge create src/Exploit.sol:Exploit -r $RPC_URL --private-key $PK --constructor-args $SETUP_ADDR
[⠊] Compiling...
[⠢] Compiling 3 files with Solc 0.8.28
[⠰] Solc 0.8.28 finished in 124.68ms
Compiler run successful!
Warning: Dry run enabled, not broadcasting transaction

Contract: Exploit
Transaction: {
  "from": "0x16b395ba08243840e31c9ace09efcbfc1b4981a7",
  "to": null,
  "maxFeePerGas": "0x3b9aca00",
  "maxPriorityFeePerGas": "0x3b9aca00",
  "gas": "0x2d846",
  "input": "0x60a060405234801561000f575f5ffd5b506040516103fa3803806103fa83398181016040528101906100319190610194565b805f5f6101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505f5f9054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16630576fd8b6040518163ffffffff1660e01b8152600401602060405180830381865afa1580156100d9573d5f5f3e3d5ffd5b505050506040513d601f19601f820116820180604052508101906100fd91906101fa565b73ffffffffffffffffffffffffffffffffffffffff1660808173ffffffffffffffffffffffffffffffffffffffff168152505050610225565b5f5ffd5b5f73ffffffffffffffffffffffffffffffffffffffff82169050919050565b5f6101638261013a565b9050919050565b61017381610159565b811461017d575f5ffd5b50565b5f8151905061018e8161016a565b92915050565b5f602082840312156101a9576101a8610136565b5b5f6101b684828501610180565b91505092915050565b5f6101c982610159565b9050919050565b6101d9816101bf565b81146101e3575f5ffd5b50565b5f815190506101f4816101d0565b92915050565b5f6020828403121561020f5761020e610136565b5b5f61021c848285016101e6565b91505092915050565b6080516101be61023c5f395f607601526101be5ff3fe608060405234801561000f575f5ffd5b5060043610610034575f3560e01c80630576fd8b14610038578063ba0bba4014610056575b5f5ffd5b610040610074565b60405161004d9190610136565b60405180910390f35b61005e610098565b60405161006b919061016f565b60405180910390f35b7f000000000000000000000000000000000000000000000000000000000000000081565b5f5f9054906101000a900473ffffffffffffffffffffffffffffffffffffffff1681565b5f73ffffffffffffffffffffffffffffffffffffffff82169050919050565b5f819050919050565b5f6100fe6100f96100f4846100bc565b6100db565b6100bc565b9050919050565b5f61010f826100e4565b9050919050565b5f61012082610105565b9050919050565b61013081610116565b82525050565b5f6020820190506101495f830184610127565b92915050565b5f61015982610105565b9050919050565b6101698161014f565b82525050565b5f6020820190506101825f830184610160565b9291505056fea264697066735822122051f95c360e80cadd2ed19074757cc3755725f6c44d50eff4695e2981473d50ce64736f6c634300081c00330000000000000000000000009954160f4f82eb263f2f81599da2c36ba62aa0e4",
  "nonce": "0x0",
  "chainId": "0x7a69"
}
ABI: [
  {
    "type": "constructor",
    "inputs": [
      {
        "name": "_setup",
        "type": "address",
        "internalType": "address"
      }
    ],
    "stateMutability": "nonpayable"
  },
  {
    "type": "function",
    "name": "GU",
    "inputs": [],
    "outputs": [
      {
        "name": "",
        "type": "address",
        "internalType": "contract GearingUp"
      }
    ],
    "stateMutability": "view"
  },
  {
    "type": "function",
    "name": "setup",
    "inputs": [],
    "outputs": [
      {
        "name": "",
        "type": "address",
        "internalType": "contract Setup"
      }
    ],
    "stateMutability": "view"
  }
]

Warning: To broadcast this transaction, add --broadcast to the previous command. See forge create --help for more.
```

UPDATE! I can't solve this challenge with foundry and I dont even know why. Already seek help in this matter. So for now I'll be solving this using foundpy via jupyter-notebook.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
import "./GearingUp.sol";

contract Attack {
    GearingUp public GU;

    constructor(address _GU) payable{
        GU = GearingUp(_GU);
        GU.callThis();
        GU.sendMoneyHere{value: 5 ether}();
        GU.withdrawReward();
        GU.sendSomeData("GearNumber1", 687221, bytes4(0x1a2b3c4d), address(this));
        GU.completedGearingUp();
    }

    function isSolved() public view returns(bool){
        return GU.allFinished();
    }
}
```

<figure><img src="../../.gitbook/assets/Screenshot (32).png" alt=""><figcaption></figcaption></figure>

Flag: ENUMA{Equipment\_up\_it\_is\_time\_to\_take\_down\_the\_CASINO}
