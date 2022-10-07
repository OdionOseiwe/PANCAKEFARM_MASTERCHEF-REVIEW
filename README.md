# PanCake Farming 🥞

## PANCAKE SWAP MASTERCHEF

What is pancakeswap?
 it's a decentralized exchange on Binance smart chain that allows users to exchange tokens for cake. It works on the automated market maker (AMM) model instead of the traditional market model, in which there is a seller and a buyer. In the AMM model, traders trade digital assets using permissionless liquidity pools. The standard for tokens is BEP20, just like the way we have EIP20 on the Ethereum blockchain.

The cake is pancakeswap native token. it's called a liquidity provider token .it’s being given to the liquidity provider when they provide liquidity to a pool.
Yield farming is a method of Staking cryptocurrency from your crypto holdings to yield more earnings
Chefs in pancakeswap are the developers Syrups pools are another type of liquidity pool that is more rewarding, this is where you stake cake to earn rewards, when you stake your tokens you are farming and when you are earning rewards you are harvesting There are two types of pools on pancakeswap special farm pools where only whitelisted stakers can stake cake and regular pools where anyone can come and stake 
The masterchef is concerning with yield farming , staking cake. Its distributes cake to pools 


PancakeSwap MasterChef v2 is a new main staking contract for Farms while providing more flexibility for adjusting the $CAKE emissions, including CAKE pool, burn and other PancakeSwap products. 
	Having gotten an intro to pancakeswap and what MasterChef those let's go into the contract.

Pragma solidity:    
```solidity
	pragma solidity 0.6.12;
```
The pragma keyword is used to enable certain compiler features or checks in this contract and it uses version 0.6.12

Imported libraries

```solidity
	import '@pancakeswap/pancake-swap-lib/contracts/math/SafeMath.sol';
```

### SafeMath.sol:
 The SafeMath library validates if an arithmetic operation would result in an integer overflow/underflow. If it would, the library throws an exception, effectively reverting the transaction. All the functions in SafeMath are internal functions and returns a value.
Its has functions that add , subtract , divide, multiply , modules, get the square root of a number 

Function add(): this function takes in uint256 a and uint256 b and string  errorMessage in memory  as parameters  its an internal function, pure and doesn't modify state and returns uint256 adds them together and returns the addition of the two number and check if the result is greaterthanorEqualto to uint a , 
If this returns true the exceution passes and return the result C but if not it gives the error message 

```solidity
	function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, 'SafeMath: addition overflow');

        return c;
    }
```

Function sub(): this function takes in uint256 a  and uint256 b and string  errorMessage in memory  as parameters  its an internal function, pure and doesn't modify state and returns uint256 subtracts uint256 a from uint256 b and checks if uint256 b is lessthanorEqualto uint256 a if true its passes and returns c but if false it reverts with an errorMessage

```solidity
function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }
```

Function mul(): this function takes in two parameters uint256 a and uint256 b   its an internal function, pure and doesn't modify state and returns uint256. First, it checks if uint256 a is equal to zero and returns 0 but, if not it passes and multiplies them together  after the multiplication it requires that the result when divided  by uint a is equal to uint256 b, if true is returns the result, if not there is an overflow and the execution reverts 


```solidity
   function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, 'SafeMath: multiplication overflow');

        return c;
    }
```

Function div(): this function takes in three parameters uint256 a and uint256 b and string errorMessage in memory its an internal function, pure and doesn't modify state and returns uint256. It require the divisor uint256 b is greater than 0 and return the result of dividing uint256 a by uint256 b else throw errorMessage 

```solidity
  function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
```

Function mod(): this function takes in three parameters uint256 a and uint256 b and string errorMessage in memory, its an internal function, pure and doesn't modify state and returns uint256. And requires that uint256 b is not 0 and get the module of uint256 a and uint256 b else it throws errorMessage


```solidity
  function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
```


Function min(): this function takes in two parameters uint256 x and uint256 y  its an internal function, pure and doesn't modify state and returns uint256 z. Checks if x is greater than y and return x else returns y 

```solidity
function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }
```

Function sqrt(): this functions takes in uint256 y and  uint256 z , calculates the square root of y and returns z

```solidity
  function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
```


### IBEP.SOL AND SAFEBEP.SOL:


```solidity
import '@pancakeswap/pancake-swap-lib/contracts/token/BEP20/IBEP20.sol';
import '@pancakeswap/pancake-swap-lib/contracts/token/BEP20/SafeBEP20.sol';
```


These contracts are tokens standards the first IBEP20.sol is the interface from BEP20.sol. it has functions which are external are does not implement them. an interface is a guide used to implement a standard or it's used to interact with other contracts

```solidity
import '../../access/Ownable.sol';
import '../../GSN/Context.sol';
import './IBEP20.sol';
import '../../math/SafeMath.sol';
import '../../utils/Address.sol';
```

BEP.sol is a contract that inheritances context.sol, IBEP.sol which is an interface that BEP.sol must all functions , Ownable.sol

```solidity
	contract BEP20 is Context, IBEP20, Ownable
```

```solidity
	using Address for address;
```

Is a way of using libraries where the Address is the name of the library and the address is a variable type

```solidity
	using SafeMath for uint256;
```

Is a way of using libraries where SafMath is the name of the library and uint256 is the variable type

```solidity
	 mapping(address => uint256) private _balances;
```

This is a mapping called _balances that uses the key address of a user to query the balance of the user and its keeps track of the address to uint256,private it can only be accessed by the contract itself and no other contract that inheritances it can call it 

```solidity
`	    mapping(address => mapping(address => uint256)) private _allowances;
```


 Its a private variable that tracks use from address which is the owner of a token and address is the spender of the token youwant yo approve and the uint256 is the amount of tokens to the allocated to BEP.sol


```solidity
	uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
```


Totalsupply : a private variables that keeps track of the total amount of tokens in circulation
_name : a private variable that stores the name of the token
_symbol : aprivate variable that stores the symbol of the token
_decimals : a private variable that stores the decimal in which this token will be in

```solidity
constructor(string memory name, string memory symbol) public {
        _name = name;
        _symbol = symbol;
        _decimals = 18;
    }
```


constructor is used for initializing variables like its sets the name of the token and symbol

```solidity
	 function getOwner() external override view returns (address) {
        return owner();
    }
```

Function getOwer():
This function is external it overrides a function called getOwner in ownable. Sol its view function its reads from state and returns the address of the owner of the toke

```solidity
	 function name() public override view returns (string memory) {
        return _name;
    }
```

Function name():

this function  is public it overrides a function in IBEP.sol called name its view function its reads from state and name returns the name of the token

```solidity
	function decimals() public override view returns (uint8) {
        return _decimals;
    }
```

Function decimals():

This  function  is public it overrides a function in IBEP.sol called decimal its view function its reads from state and returns the number of decimals this token uses


```solidity
\  function symbol() public override view returns (string memory) {
        return _symbol;
    }
```

Function symbol():
This  function  is publicit overrides a function in IBEP.sol called symbol its view function its reads from state and returns the symbol of the token


```solidity
  function totalSupply() public override view returns (uint256) {
        return _totalSupply;
    }
```

Function totalsupply():

This  function  is public it overrides a function in IBEP.sol called  totalsupply and its view function its reads from state and returns the totalsupply of a the token


```solidity
  function balanceOf(address account) public override view returns (uint256) {
        return _balances[account];
    }
```

Function balanceOf():

This function is called balanceof is takes in address account as a parameter its public, its overrides balanceOf in IBEP.sol and given the key which is the address it can query the balance of an address

```solidity
  function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), 'BEP20: transfer from the zero address');
        require(recipient != address(0), 'BEP20: transfer to the zero address');

        _balances[sender] = _balances[sender].sub(amount, 'BEP20: transfer amount exceeds balance');
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }
```

Function _transfer():

Function _transfer takes in address of the sender (the owner of the tokens) , the recipient of the tokens and the amount to send , its internal .  function _transfer requires that the sender is not address Zero , requires that the recipient is not address zero , so not to send tokens to a null address (an address without a private)   . subtracts the number of tokens to be sent from the sender and adds the amount to the recipient an emits an event called Transfer .


```solidity
   function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal {
        require(owner != address(0), 'BEP20: approve from the zero address');
        require(spender != address(0), 'BEP20: approve to the zero address');

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
```

Function _approve(): 

Takes address owner, address spender and amount of tokens to be approved its internal  function _approve requires that the sender is not address Zero , requires that the recipient is not address zero , so not to send tokens to a null address (an address without a private) and updates the _allowances mapping and emit event Approval

```solidity
 function allowance(address owner, address spender) public override view returns (uint256) {
        return _allowances[owner][spender];
    }
```

Function allowance():
Its a public function, overrides allowances in IBEP.sol, its a view function and returns bool if this spenders has been allowed or not. Takes address of spender and owner and given checks if the spender has allowed or not

```solidity
  function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
```

Function approve():
It is a public function, overrides allowances in IBEP.sol, is a view function and returns a bool if these spenders have been allowed or not. Takes the address of the spender and the number of tokens that were allowed. its  called _approve and checks if a user has been approved it returns true else false

```solidity
	  function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(amount, 'BEP20: transfer amount exceeds allowance')
        );
        return true;
    }
```

function transferFrom(): 

This function takes in address sender, address recipient,uint256 amount and is public its overrides a function is IBEP.sol and returns  bool . it calls _transfer() and inputs sender, recipient and amount and calls _approve() and checks if the recipient has been approved and checks if the amount the recipient is inputing is greater than or equal to the approved amount


```solidity
	  function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }
```

function increaseAllowance():

With this function the msg.sender who has approved a spender before can increase the amount . it takes in the address spender, uint256 addedValue , where the spender is the approved address and the addedValue is the amount to be added to the formal


```solidity
   function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(subtractedValue, 'BEP20: decreased allowance below zero')
        );
        return true;
    }
```

function decreaseAllowance():

With this function the msg.sender who has approved a spender before can decrease the amount. it takes in yhe address spender, uint256 subtractedValue , where the spender is the approved address and the subtractedValue is the amount to be subtracted to the formal

```solidity
   function _mint(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: mint to the zero address');

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }
```

function _mint();

The mint function, this where all the tokens are been generated.
It takes in address account, uint256 amount it's internal (can only be called by this contract and other derived contracts) . its first checks that the account is not address zero(because if tokens are minted to this address "address zero" is cant be gotten again), increases total supply, increases the account balance by calling _balances and lastly emits an event called Transfer().

```solidity
   function mint(uint256 amount) public onlyOwner returns (bool) {
        _mint(_msgSender(), amount);
        return true;
    }
```

function mint():

This function is public its called by only the owner of the contract and it accepts the uint256 amount and returns a bool. In the logic part, it calls _mint() and mints tokens to the owner of the contract and returns true when it's successful.

```solidity
   function _burn(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: burn from the zero address');

        _balances[account] = _balances[account].sub(amount, 'BEP20: burn amount exceeds balance');
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }
```

function _burn():

where the burning of tokens occurs, with this function you can deduce the total circulating supply of tokens, it takes in two parameters address account, uint256 amount and its visibility is internal, where the account is the address of the person that wants to burn the token and the amount is the number of tokens the person wants to burn . it requires that account that wants to burn the tokens is not address zero , removes the amount from the account address and deduces total supply and emits a Transfer event.

```solidity
	function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(
            account,
            _msgSender(),
            _allowances[account][_msgSender()].sub(amount, 'BEP20: burn amount exceeds allowance')
        );
    }
```

Destroys amount tokens from account, amount is then deducted from the caller's allowance 



### OWNABLE.SOL

```solidity
import '@pancakeswap/pancake-swap-lib/contracts/access/Ownable.sol';
```

This contract imports Context.sol and inheritances it
```solidity
import '../GSN/Context.sol';
```

```solidity
    address private _owner;
```

Owner a state variable and holds the owner of the contract and its private

```solidity
	event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```
    
	event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
An event called OwnershipTransferred() takes address previousOwner and address newOwner its emitted when the ownership of a contract is being transferred .

```solidity
	constructor() internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }
```
Initializes the contract setting the deployer as the initial owner
and emits OwnershipTransferred and here ownership is being transferred from address(0) to msg.sender.

```solidity
	 function owner() public view returns (address) {
        return _owner;
    }
```
Returns the address of the current owner.

```solidity
	    modifier onlyOwner() {
        require(_owner == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }
```
Throws if called by any account other than the owner.

```solidity
	function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
```

Leaves the contract without owner. It will not be possible to call
onlyOwner functions anymore. Can only be called by the current owner. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.


```solidity
	function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }
```

Transfers ownership of the contract to a new account newOwner.
Can only be called by the current owner.


```solidity
	function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
```

this function is internal and takes in the address of the new owner and checks if not address(0) and first emits OwnershipTransferred() and updates the state.

```solidity
	import "./CakeToken.sol";
	import "./SyrupBar.sol";
```

### CAKETOKEN.SOL
### SYRUPBAR.SOL
__

```solidity
pragma solidity 0.6.12;

```
The pragma keyword is used to enable certain compiler features or checks in this contract and it uses version 0.6.12


```solidity
import "@pancakeswap/pancake-swap-lib/contracts/token/BEP20/BEP20.sol";

```

```solidity
contract CakeToken is BEP20('PancakeSwap Token', 'Cake') 

```

Its contract inheritance's BEP20 and initialises the constructor with 'PancakeSwap Token' as the name and 'Cake' as the symbol'


```solidity
 function mint(address _to, uint256 _amount) public onlyOwner {
        _mint(_to, _amount);
        _moveDelegates(address(0), _delegates[_to], _amount);
    }

```

With this function its mints some amount of tokens to the delegatee and and moves delegation address(0).

```solidity
mapping (address => address) internal _delegates;

```

saves to a mapping a record of each accounts delegate

```solidity
    struct Checkpoint {
        uint32 fromBlock;
        uint256 votes;
    }
```

a struct that saves the block number and number of votes


```solidity
mapping (address => mapping (uint32 => Checkpoint)) public checkpoints;
```

maps address to index to struct

```solidity
mapping (address => uint32) public numCheckpoints;
```

this mapping address to number of votes


```solidity
bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");
```

this is from EIP 712 the hash of a  unique string  

```solidity
bytes32 public constant DELEGATION_TYPEHASH = keccak256("Delegation(address delegatee,uint256 nonce,uint256 expiry)");
```

this is from EIP 712 the hash of a  unique string  


```solidity
    mapping (address => uint) public nonces;
```

keeps track of an address to the number of times he delegates


```solidity
   event DelegateChanged(address indexed delegator, address indexed fromDelegate, address indexed toDelegate);  
```

An event thats emitted when an account changes its delegate

```solidity
  event DelegateVotesChanged(address indexed delegate, uint previousBalance, uint newBalance);
```

An event thats emitted when a delegate account's vote balance changes


```solidity
  function delegates(address delegator)
        external
        view
        returns (address)
    {
        return _delegates[delegator];
    }
```

its returns the address of the delegatee that this address delegated

```solidity
   function delegate(address delegatee) external {
        return _delegate(msg.sender, delegatee);
    }
```

this function Delegate votes from msg.sender which is the delegator to `delegatee 

```solidity
   function delegateBySig(
        address delegatee,
        uint nonce,
        uint expiry,
        uint8 v,
        bytes32 r,
        bytes32 s
    )
        external
    {
        bytes32 domainSeparator = keccak256(
            abi.encode(
                DOMAIN_TYPEHASH,
                keccak256(bytes(name())),
                getChainId(),
                address(this)
            )
        );

        bytes32 structHash = keccak256(
            abi.encode(
                DELEGATION_TYPEHASH,
                delegatee,
                nonce,
                expiry
            )
        );

        bytes32 digest = keccak256(
            abi.encodePacked(
                "\x19\x01",
                domainSeparator,
                structHash
            )
        );

        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "CAKE::delegateBySig: invalid signature");
        require(nonce == nonces[signatory]++, "CAKE::delegateBySig: invalid nonce");
        require(now <= expiry, "CAKE::delegateBySig: signature expired");
        return _delegate(signatory, delegatee);
    }
```
address delegatee,uint nonce,uint expiry, uint8 v,bytes32 r, bytes32 s and hash the message and recover the address of who signed the message


```solidity
  function getCurrentVotes(address account)
        external
        view
        returns (uint256)
    {
        uint32 nCheckpoints = numCheckpoints[account];
        return nCheckpoints > 0 ? checkpoints[account][nCheckpoints - 1].votes : 0;
    }
```
 Gets the current votes balance for account


```solidity
  function getPriorVotes(address account, uint blockNumber)
        external
        view
        returns (uint256)
    {
        require(blockNumber < block.number, "CAKE::getPriorVotes: not yet determined");

        uint32 nCheckpoints = numCheckpoints[account];
        if (nCheckpoints == 0) {
            return 0;
        }

        // First check most recent balance
        if (checkpoints[account][nCheckpoints - 1].fromBlock <= blockNumber) {
            return checkpoints[account][nCheckpoints - 1].votes;
        }

        // Next check implicit zero balance
        if (checkpoints[account][0].fromBlock > blockNumber) {
            return 0;
        }

        uint32 lower = 0;
        uint32 upper = nCheckpoints - 1;
        while (upper > lower) {
            uint32 center = upper - (upper - lower) / 2; // ceil, avoiding overflow
            Checkpoint memory cp = checkpoints[account][center];
            if (cp.fromBlock == blockNumber) {
                return cp.votes;
            } else if (cp.fromBlock < blockNumber) {
                lower = center;
            } else {
                upper = center - 1;
            }
        }
        return checkpoints[account][lower].votes;
    }

```

it gets all the votes of this account

```solidity
  function _delegate(address delegator, address delegatee)
        internal
    {
        address currentDelegate = _delegates[delegator];
        uint256 delegatorBalance = balanceOf(delegator); // balance of underlying CAKEs (not scaled);
        _delegates[delegator] = delegatee;

        emit DelegateChanged(delegator, currentDelegate, delegatee);

        _moveDelegates(currentDelegate, delegatee, delegatorBalance);
    }
```

its changes delegate 



```solidity
  function _moveDelegates(address srcRep, address dstRep, uint256 amount) internal {
        if (srcRep != dstRep && amount > 0) {
            if (srcRep != address(0)) {
                // decrease old representative
                uint32 srcRepNum = numCheckpoints[srcRep];
                uint256 srcRepOld = srcRepNum > 0 ? checkpoints[srcRep][srcRepNum - 1].votes : 0;
                uint256 srcRepNew = srcRepOld.sub(amount);
                _writeCheckpoint(srcRep, srcRepNum, srcRepOld, srcRepNew);
            }

            if (dstRep != address(0)) {
                // increase new representative
                uint32 dstRepNum = numCheckpoints[dstRep];
                uint256 dstRepOld = dstRepNum > 0 ? checkpoints[dstRep][dstRepNum - 1].votes : 0;
                uint256 dstRepNew = dstRepOld.add(amount);
                _writeCheckpoint(dstRep, dstRepNum, dstRepOld, dstRepNew);
            }
        }
    }
```

its moves delegates from an old delegatee to a new delegatee


```solidity
interface IMigratorChef {
    function migrate(IBEP20 token) external returns (IBEP20);
}
```

Perform LP token migration from legacy PancakeSwap to CakeSwap.

```solidity
interface IMigratorChef {
    using SafeMath for uint256;
    using SafeBEP20 for IBEP20;
}
```
safeMath is a library that used for uint256

SafeBEP20 is an interface that will be used to interact 

```solidity
struct UserInfo {
    uint256 amount;    
	uint256 rewardDebt;
}
```

a struct that contains users amount and rewardDebt