PROTOCOL NAME: DEX.AG
CATEGORY: DEFI
SMART CONTRACT: DEX.AG

FUNCTION NAME:   callOptionalReturn()

BLOCK EXPLORER LINK: https://etherscan.io/address/0x745daa146934b27e3f0b6bff1a6e36b9b90fb131#code

FUNCTION CODE:
```
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves.

        // A Solidity high level call has three parts:
        //  1. The target address is checked to verify it contains contract code
        //  2. The call itself is made, and success asserted
        //  3. The return value is decoded, which in turn checks the size of the returned data.
        // solhint-disable-next-line max-line-length
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}
```

LOW LEVEL METHOD USED:
.call()

FUNCTION PURPOSE:
The function uses a low level solidity function call to imitate a high level function call this way, it relaxes the requirement on the return value & bypassing the solidity data size checking  mechanism to achieve an optional return

DETAILED USAGE:
The function uses the call method to assert the passing of parameters and returning a boolean value, this way, only a truthy/falsey value can be returned which can be optionally checked for instead of solidity having to "verify" the entire data return type

IMPACT:
This function is ensures that the address of the ERC20 passed to the contract is in itself a contract first, it then performs an imitation of a high level call to bypass solidity return type limit, ensuring success and optimizing on gas without much of a security tradeoff/compromise. It stresses the importance of low-level calls in performing advanced activities that may require more "fine-tuning", overall in the contract, it provides an optional return value for a call, performing the aforementioned checks.
