/**
 *Submitted for verification at BscScan.com on 2021-06-27
*/

// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.5;

library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain`call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data)
        internal
        returns (bytes memory)
    {
        return functionCall(target, data, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return
            functionCallWithValue(
                target,
                data,
                value,
                "Address: low-level call with value failed"
            );
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(
            address(this).balance >= value,
            "Address: insufficient balance for call"
        );
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value: value}(
            data
        );
        return _verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data)
        internal
        view
        returns (bytes memory)
    {
        return
            functionStaticCall(
                target,
                data,
                "Address: low-level static call failed"
            );
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data)
        internal
        returns (bytes memory)
    {
        return
            functionDelegateCall(
                target,
                data,
                "Address: low-level delegate call failed"
            );
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) private pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

interface IBEP20 {
    // Functions

    function totalSupply() external view returns (uint256); // Returns the amount of tokens in existence.

    function decimals() external view returns (uint8); // Returns the token decimals.

    function symbol() external view returns (string memory); // Returns the token symbol.

    function name() external view returns (string memory); // Returns the token name.

    function getOwner() external view returns (address); // Returns the bep token owner.

    function balanceOf(address account) external view returns (uint256); // Returns the amount of tokens owned by `account`

    function transfer(address recipient, uint256 amount)
        external
        returns (bool); // transfer tokens to addr, Emits a {Transfer} event.

    function allowance(address _owner, address spender)
        external
        view
        returns (uint256); // Returns remaining tokens that spender is allowed during {approve} or {transferFrom}

    function approve(address spender, uint256 amount) external returns (bool); // sets amount of allowance, emits approval event

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool); // move amount, then reduce allowance, emits a transfer event

    // Events

    event Transfer(address indexed from, address indexed to, uint256 value); // emitted when value tokens moved, value can be zero

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    ); // emits when allowance of spender for owner is set by a call to approve. value is new allowance
}

library SafeBEP20 {
    using Address for address;

    function safeTransfer(
        IBEP20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(
            token,
            abi.encodeWithSelector(token.transfer.selector, to, value)
        );
    }

    function safeTransferFrom(
        IBEP20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(
            token,
            abi.encodeWithSelector(token.transferFrom.selector, from, to, value)
        );
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IBEP20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeBEP20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(
            token,
            abi.encodeWithSelector(token.approve.selector, spender, value)
        );
    }

    function safeIncreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender) + value;
        _callOptionalReturn(
            token,
            abi.encodeWithSelector(
                token.approve.selector,
                spender,
                newAllowance
            )
        );
    }

    function safeDecreaseAllowance(
        IBEP20 token,
        address spender,
        uint256 value
    ) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(
                oldAllowance >= value,
                "SafeBEP20: decreased allowance below zero"
            );
            uint256 newAllowance = oldAllowance - value;
            _callOptionalReturn(
                token,
                abi.encodeWithSelector(
                    token.approve.selector,
                    spender,
                    newAllowance
                )
            );
        }
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IBEP20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(
            data,
            "SafeBEP20: low-level call failed"
        );
        if (returndata.length > 0) {
            // Return data is optional
            // solhint-disable-next-line max-line-length
            require(
                abi.decode(returndata, (bool)),
                "SafeBEP20: BEP20 operation did not succeed"
            );
        }
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract DADDY is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using SafeBEP20 for IBEP20;

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isAdminAccount;

    mapping(address => bool) private _isExcluded;
    address[] private _excluded;

    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal = 420 * 10**6 * 10**9;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));
    uint256 private _tFeeTotal;

    string private _name = "DADDY";
    string private _symbol = "$DADDY";
    uint8 private _decimals = 9;

    // uint256 public _taxFee = 1;
    uint256 public _taxFee = 5;
    uint256 private _previousTaxFee = _taxFee;

    uint256 public _convertBNBFee = 10;
    // uint256 public _convertBNBFee = 7;
    uint256 private _previousConvertBNBFee = _convertBNBFee;
    uint256 public rconvertBNBAmountTrackingSell = 0;
    uint256 public tconvertBNBAmountTrackingSell = 0;

    mapping(address => uint256) public timePurchased;
    mapping(address => uint256) public timeGained;
    mapping(address => uint256) public timeToReward;
    address[] public addressesToReward;
    mapping(address => bool) public isAddressExemptFromBNBRewards;
    // uint256 public bnbBuyResetThresholdPercent = 30;
    uint256 public bnbBuyResetThresholdPercent = 5;
    uint256 public supplyAmountCompareForBNBRewards = 420 * 10**6 * 10**9;

    uint256 public bnbMinimumAmountInContractToReward = 1000000000000000000; // minimum 1 BNB    // CHANGEIT - make sure this is set to 1 BNB whcih is 1000000000000000000
    // uint256 public bnbMinimumAmountInContractToReward = 100000000000000000;     // 0.1 BNB for testing

    // uint256 public bnbMaxUntilCharityCut = 10000000;     // minimum 1 BNB        // CHANGEIT - make sure it's set to 1 BNB of course
    uint256 public bnbMaxUntilCharityCut = 1000000000000000000; // minimum 1 BNB        // CHANGEIT - make sure it's set to 1 BNB of course
    uint256 public percentOfCharityCut = 1;
    uint256 public marketingPrecent = 3;

    uint256 public maxNumOfTransfersToDoForReward = 10;

    uint256 public minimumAmountToHoldForRewards = 4200 * 10**9; // must hold at least  0.0001% of the token to get rewards

    // uint256 public hoursToAddForReward = 30 minutes; // CHANGEIT - make sure this is properly set
    uint256 public hoursToAddForReward = 60 minutes;

    bool public isRewardSystemEnabled = false;

    address public charityWallet = 0xbFCCbc3C7D1C3D5A6E1bFDe218D97CEe7121624d; // CHANGEIT - charity
    address public marketingWallet = 0x921F24C451Ae8f343E86d777bA3e626c7798D161;

    address public deadAddressOne = 0x0000000000000000000000000000000000000000;
    address public deadAddressTwo = 0x0000000000000000000000000000000000000001;
    address public deadAddressThree =
        0x000000000000000000000000000000000000dEaD;

    // uint256 public _liquidityFee = 3;
    uint256 public _liquidityFee = 4;
    uint256 private _previousLiquidityFee = _liquidityFee;
    uint256 public rliquidityAmountTrackingSell = 0;
    uint256 public tliquidityAmountTrackingSell = 0;
    uint256 private _rewardedAmount = 0;
    uint256 public _maxTxAmount = 420000 * 10**9; //0.1% of totalSupply

    uint256 public bnbAmountForBNBRewards = 0;
    uint256 public bnbAmountForLIQ = 0;

    bool public inDistributeRewards;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;
    uint256 public numTokensSellToAddToLiquidity = 4200000 * 10**9; // 0.03% - safemoon's is 0.05%
    bool inSwapAndLiquify;
    bool public isSwapAndLiquifyEnabled = false;

    event Purchase(address indexed to, uint256 amount);
    event SwapAndLiquifyEnabledUpdated(bool enabled);
    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiquidity
    );

    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor() {
        _rOwned[_msgSender()] = _rTotal;

        //For PCS MAINNET: 0x10ED43C718714eb63d5aA57B78B54704E256024E
        // For PCS TESTNET PAIR ISSUE: 0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3    -- GOOD ONE
        //For PCS TESTNET: 0xD99D1c33F9fC3444f8101754aBC46c52416550D1
        //For UNI RINKEBY: 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        address routerDEXAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E; // TODO - Change this to the right router
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            routerDEXAddress
        );
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
        .createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;

        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[charityWallet] = true;
        emit Transfer(address(0), _msgSender(), _tTotal);
        _isAdminAccount[owner()] = true;
        _isAdminAccount[charityWallet] = true;
        _isAdminAccount[address(this)] = true;

        isAddressExemptFromBNBRewards[uniswapV2Pair] = true;
        isAddressExemptFromBNBRewards[_uniswapV2Router.factory()] = true;
        isAddressExemptFromBNBRewards[routerDEXAddress] = true;
        isAddressExemptFromBNBRewards[deadAddressOne] = true;
        isAddressExemptFromBNBRewards[deadAddressTwo] = true;
        isAddressExemptFromBNBRewards[deadAddressThree] = true;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function totalFees() public view returns (uint256) {
        return _tFeeTotal;
    }

    function reflectionFromToken(uint256 tAmount, bool deductTransferFee)
        public
        view
        returns (uint256)
    {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferFee) {
            (uint256 rAmount, , , , , , ) = _getValues(tAmount);
            return rAmount;
        } else {
            (, uint256 rTransferAmount, , , , , ) = _getValues(tAmount);
            return rTransferAmount;
        }
    }

    function tokenFromReflection(uint256 rAmount)
        public
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount must be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount.div(currentRate);
    }

    function excludeFromReward(address account) public onlyOwner() {
        require(!_isExcluded[account], "Account is already excluded");
        if (_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function includeInReward(address account) external onlyOwner() {
        require(_isExcluded[account], "Account is already excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _tOwned[account] = 0;
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
    }

    function addAdminAccount(address account) external onlyOwner() {
        _isAdminAccount[account] = true;
    }

    function removeAdminAccount(address account) external onlyOwner() {
        _isAdminAccount[account] = false;
    }

    function excludeFromFee(address account) external onlyOwner() {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) external onlyOwner() {
        _isExcludedFromFee[account] = false;
    }

    function setTaxFeePercent(uint256 taxFee) external onlyOwner() {
        require(taxFee <= 5, "Percentage too high. Please use a lower fee.");
        _taxFee = taxFee;
    }

    function setConvertBNBFeePercent(uint256 newConvertBNBFee)
        external
        onlyOwner()
    {
        require(
            newConvertBNBFee <= 15,
            "Percentage too high. Please use a lower fee."
        );
        _convertBNBFee = newConvertBNBFee;
    }

    function setLiquidityFeePercent(uint256 liquidityFee) external onlyOwner() {
        require(
            liquidityFee <= 5,
            "Percentage too high. Please use a lower fee."
        );
        _liquidityFee = liquidityFee;
    }

    function setSwapAndLiquifyEnabled(bool _enabled) external onlyOwner() {
        isSwapAndLiquifyEnabled = _enabled;
        emit SwapAndLiquifyEnabledUpdated(_enabled);
    }

    function setMaxTxAmount(uint256 maxTxAmount) external onlyOwner() {
        _maxTxAmount = _tTotal.mul(maxTxAmount).div(10**2);
    }

    function setMaxTxAmountNumber(uint256 newMaxTxAmount) external onlyOwner() {
        _maxTxAmount = newMaxTxAmount;
    }

    function _reflectFee(uint256 rFee, uint256 tFee) private {
        _rTotal = _rTotal.sub(rFee);
        _tFeeTotal = _tFeeTotal.add(tFee);
    }

    function _getValues(uint256 tAmount)
        private
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        (
            uint256 tTransferAmount,
            uint256 tFee,
            uint256 tBNB,
            uint256 tLiquidity
        ) = _getTValues(tAmount);
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFee) = _getRValues(
            tAmount,
            tFee,
            tBNB,
            _getRate()
        );
        return (
            rAmount,
            rTransferAmount,
            rFee,
            tTransferAmount,
            tFee,
            tBNB,
            tLiquidity
        );
    }

    function _getTValues(uint256 tAmount)
        private
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 tFee = calculateTaxFee(tAmount);
        uint256 tBNB = calculateConvertBNBFee(tAmount);
        uint256 tLiquidity = calculateLiquidityFee(tAmount);
        uint256 tTransferAmount = tAmount.sub(tFee).sub(tBNB).sub(tLiquidity);
        return (tTransferAmount, tFee, tBNB, tLiquidity);
    }

    function _getRValues(
        uint256 tAmount,
        uint256 tFee,
        uint256 tBNB,
        uint256 currentRate
    )
        private
        pure
        returns (
            uint256,
            uint256,
            uint256
        )
    {
        uint256 rAmount = tAmount.mul(currentRate);
        uint256 rFee = tFee.mul(currentRate);
        uint256 rBNB = tBNB.mul(currentRate);
        uint256 rTransferAmount = rAmount.sub(rFee).sub(rBNB);
        return (rAmount, rTransferAmount, rFee);
    }

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (
                _rOwned[_excluded[i]] > rSupply ||
                _tOwned[_excluded[i]] > tSupply
            ) return (_rTotal, _tTotal);
            rSupply = rSupply.sub(_rOwned[_excluded[i]]);
            tSupply = tSupply.sub(_tOwned[_excluded[i]]);
        }
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function _takeLiquidity(uint256 tLiquidity) private {
        uint256 currentRate = _getRate();
        uint256 rLiquidity = tLiquidity.mul(currentRate);
        _rOwned[address(this)] = _rOwned[address(this)].add(rLiquidity);
        rliquidityAmountTrackingSell += rLiquidity;
        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] = _tOwned[address(this)].add(tLiquidity);
            tliquidityAmountTrackingSell += tLiquidity;
        }
    }

    function _takeBNB(uint256 tBNB) private {
        uint256 currentRate = _getRate();
        uint256 rBNB = tBNB.mul(currentRate);
        _rOwned[address(this)] = _rOwned[address(this)].add(rBNB);
        rconvertBNBAmountTrackingSell += rBNB;
        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] = _tOwned[address(this)].add(tBNB);
            tconvertBNBAmountTrackingSell += tBNB;
        }
    }

    function calculateTaxFee(uint256 _amount) private view returns (uint256) {
        return _amount.mul(_taxFee).div(10**2);
    }

    function calculateConvertBNBFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount.mul(_convertBNBFee).div(10**2);
    }

    function calculateLiquidityFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount.mul(_liquidityFee).div(10**2);
    }

    function removeAllFee() private {
        _previousTaxFee = _taxFee;
        _previousConvertBNBFee = _convertBNBFee;
        _previousLiquidityFee = _liquidityFee;

        _taxFee = 0;
        _convertBNBFee = 0;
        _liquidityFee = 0;
    }

    function restoreAllFee() private {
        _taxFee = _previousTaxFee;
        _convertBNBFee = _previousConvertBNBFee;
        _liquidityFee = _previousLiquidityFee;
    }

    function setFeesAndSwapAndLiquifyAndRewardSystem() external onlyOwner() {
        _taxFee = 1;
        _convertBNBFee = 10;
        _liquidityFee = 4;
        isRewardSystemEnabled = true;
        isSwapAndLiquifyEnabled = true;
        _maxTxAmount = 420000 * 10**9;
    }

    function isAdminAccount(address account) public view returns (bool) {
        return _isAdminAccount[account];
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        //require(amount > 0, "Transfer amount must be greater than zero");

        uint256 balanceBeforeTransfer = balanceOf(to);

        if (
            (to == uniswapV2Pair) &&
            (!_isAdminAccount[from] || !_isAdminAccount[to])
        ) {
            require(
                amount <= _maxTxAmount,
                "Transfer amount exceeds the maxTxAmount."
            );
        }

        if (!inSwapAndLiquify) {
            // if we aren't swapping and liquifying let's do it
            // We have to change how we do this, we need to sell our tokens every single transfer
            // if it has enough for swap and liquify then pair up the eth with token amount
            // we are still selling all the tokens each and every single time
            uint256 amountToSellForLIQ = tokenFromReflection(
                rliquidityAmountTrackingSell
            );
            uint256 amountToSellForBNB = tokenFromReflection(
                rconvertBNBAmountTrackingSell
            );
            if (_isExcluded[address(this)]) {
                amountToSellForLIQ = tokenFromReflection(
                    tliquidityAmountTrackingSell
                );
                amountToSellForBNB = tokenFromReflection(
                    tconvertBNBAmountTrackingSell
                );
            }
            swapTokensForEthAndSwapAndLiquify(
                amountToSellForLIQ,
                amountToSellForBNB,
                from
            );
        }

        bool takeFee = true;

        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            takeFee = false;
        }

        _tokenTransfer(from, to, amount, takeFee);

        if (isRewardSystemEnabled) {
            if (!inSwapAndLiquify) {
                distributeBNBReward(); // distribute rewards before the transfer, this handles any rewards already needed
            }
        }

        // Tracking Balance and Rewards
        uint256 balanceAfterTransfer = balanceOf(to);

        if (!isAddressExemptFromBNBRewards[to]) {
            if (
                balanceAfterTransfer > 0 &&
                balanceAfterTransfer >= minimumAmountToHoldForRewards
            ) {
                if (timeGained[to] == 0) {
                    timeGained[to] = block.timestamp.add(hoursToAddForReward);
                    timeToReward[to] = block.timestamp.add(hoursToAddForReward);
                    addressesToReward.push(to); // set address to be rewarded
                }

                if (from == uniswapV2Pair) {
                    // if it was a buy....

                    // if amount transfered is greater than the threshold percentage given his balanceBeforeTransfer
                    if (balanceBeforeTransfer == 0) {
                        balanceBeforeTransfer = 1; // set to 1 so we don't get any errors
                    }
                    if (
                        (
                            balanceAfterTransfer.mul(100).div(
                                balanceBeforeTransfer
                            )
                        ) > bnbBuyResetThresholdPercent
                    ) {
                        timePurchased[to] = block.timestamp.add(
                            hoursToAddForReward
                        );
                        timeToReward[to] = block.timestamp.add(
                            hoursToAddForReward
                        );
                        for (uint256 i = 0; i < addressesToReward.length; i++) {
                            if (addressesToReward[i] == to) {
                                removeIndexFromRewardArray(i);
                                break;
                            }
                        }
                        addressesToReward.push(to); // places the address back of the line
                    }
                }
            }
        }
    }

    function swapTokensForEthAndSwapAndLiquify(
        uint256 amountToSellForLIQ,
        uint256 amountToSellForBNB,
        address fromAddress
    ) private lockTheSwap() {
        uint256 halfOfLIQamount = 0;
        uint256 otherHalfOfLIQamount = 0;

        // for specifically adding Liquidity
        if (
            amountToSellForLIQ >= numTokensSellToAddToLiquidity &&
            fromAddress != uniswapV2Pair &&
            isSwapAndLiquifyEnabled
        ) {
            amountToSellForLIQ = numTokensSellToAddToLiquidity;
            rliquidityAmountTrackingSell = 0;
            tliquidityAmountTrackingSell = 0; // we reset these back to 0 as the amount we want to sell is captured
            halfOfLIQamount = amountToSellForLIQ.div(2);
            otherHalfOfLIQamount = amountToSellForLIQ.sub(halfOfLIQamount);
        }

        uint256 fullAmountToSellForBNBRewards = 0;
        if (fromAddress != uniswapV2Pair && isRewardSystemEnabled) {
            fullAmountToSellForBNBRewards = amountToSellForBNB;
            rconvertBNBAmountTrackingSell = 0; // we reset these back to 0 as the amount we want to sell is captured
            tconvertBNBAmountTrackingSell = 0;
        }

        uint256 totalToSell = fullAmountToSellForBNBRewards.add(
            halfOfLIQamount
        );

        uint256 initialBalance = address(this).balance;

        uint256 contractTokenBalance = balanceOf(address(this));
        if (contractTokenBalance >= totalToSell && totalToSell > 0) {
            _approve(
                address(this),
                address(uniswapV2Router),
                totalToSell.add(otherHalfOfLIQamount)
            );

            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = uniswapV2Router.WETH();

            uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                totalToSell,
                0,
                path,
                address(this),
                block.timestamp
            );

            if (halfOfLIQamount > 0) {
                // now if we are greater than zero on the half liq amount we know we sold for BNB now we need to do the pairing.

                uint256 percentOfLIQ = halfOfLIQamount.mul(100).div(
                    totalToSell
                );
                if (percentOfLIQ == 0) {
                    percentOfLIQ = 1;
                }
                // uint256 percentOfBNB = amountToSellForBNB.mul(100).div(totalToSell);

                uint256 newBalance = address(this).balance.sub(initialBalance);

                if (newBalance.mul(percentOfLIQ).div(100) > 0) {
                    uniswapV2Router.addLiquidityETH{
                        value: newBalance.mul(percentOfLIQ).div(100)
                    }( // Add liquidity
                        address(this),
                        otherHalfOfLIQamount,
                        0,
                        0,
                        address(this),
                        block.timestamp
                    );

                    emit SwapAndLiquify(
                        halfOfLIQamount,
                        newBalance,
                        otherHalfOfLIQamount
                    );
                }
            }
        }
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
        bool takeFee
    ) private {
        if (!takeFee) {
            removeAllFee();
        }

        _transferStandard(sender, recipient, amount);

        if (!takeFee) {
            restoreAllFee();
        }
    }

    function distributeBNBReward() private {
        uint256 BNBbalanceInContractOld = address(this).balance;

        if (BNBbalanceInContractOld >= bnbMinimumAmountInContractToReward) {
            // check to make sure we have a minimum amount of BNB to do the distribution

            uint256 BNBbalanceInContract = address(this).balance;

            uint256 numberOfTransfersToDo = maxNumOfTransfersToDoForReward; // have to check and set the number of transfers to do
            if (numberOfTransfersToDo > addressesToReward.length) {
                numberOfTransfersToDo = addressesToReward.length; // in case there are a small number of holders I need to check this and set it
            }

            for (uint256 i = 0; i < numberOfTransfersToDo; i++) {
                if (block.timestamp >= timeToReward[addressesToReward[i]]) {
                    // the soonest people should be close to the start, there will be some slight variation but it should all work

                    uint256 tokenBalanceOfRewarded = balanceOf(
                        addressesToReward[i]
                    );
                    if (
                        tokenBalanceOfRewarded >= minimumAmountToHoldForRewards
                    ) {
                        // as long as the user has the minimum then it's good

                        uint256 bnbToReward = BNBbalanceInContract
                        .mul(tokenBalanceOfRewarded)
                        .div(supplyAmountCompareForBNBRewards);

                        payableAddress(addressesToReward[i]).transfer(
                            bnbToReward
                        ); // pay the address their bnb
                        // add rewarded amount
                        _rewardedAmount = _rewardedAmount + bnbToReward;

                        timeToReward[addressesToReward[i]] = block
                        .timestamp
                        .add(hoursToAddForReward); // sets the next reward time 24 hours away

                        address addressToAddBack = addressesToReward[i];

                        removeIndexFromRewardArray(i);

                        addressesToReward.push(addressToAddBack); // adds him back to the list at the end
                    } else {
                        removeIndexFromRewardArray(i);
                    }
                }
            }
            if (_rewardedAmount > 0) {
                uint256 marketingRewards = _rewardedAmount
                .mul(marketingPrecent)
                .div(100);
                uint256 charityReward = _rewardedAmount
                .mul(percentOfCharityCut)
                .div(100);

                payableAddress(charityWallet).transfer(charityReward);
                payableAddress(marketingWallet).transfer(marketingRewards);
                _rewardedAmount = 0;
            }
        }
    }

    function distributeBNBRewardManual() external {
        require(isRewardSystemEnabled, "Reward system is not enabled");
        distributeBNBReward();
    }

    function removeIndexFromRewardArray(uint256 index) private {
        if (index < addressesToReward.length) {
            for (uint256 i = index; i < addressesToReward.length - 1; i++) {
                addressesToReward[i] = addressesToReward[i + 1];
            }
            addressesToReward.pop();
        }
    }

    function removeIndexFromRewardArrayOwnerOnly(uint256 index)
        external
        onlyOwner()
    {
        removeIndexFromRewardArray(index);
    }

    function setBNBBuyResetThresholdPercent(uint256 newBNBThresholdPercent)
        external
        onlyOwner()
    {
        bnbBuyResetThresholdPercent = newBNBThresholdPercent;
    }

    function setSupplyAmountCompareForBNBRewards(
        uint256 newSupplyAmountCompareForBNBRewards
    ) external onlyOwner() {
        supplyAmountCompareForBNBRewards = newSupplyAmountCompareForBNBRewards;
    }

    function addExemptWalletFromBNBRewards(address addAddress)
        external
        onlyOwner()
    {
        isAddressExemptFromBNBRewards[addAddress] = true;
        for (uint256 i = 0; i < addressesToReward.length; i++) {
            if (addressesToReward[i] == addAddress) {
                removeIndexFromRewardArray(i);
                break;
            }
        }
    }

    function removeExemptWalletFromBNBRewards(address removeAddress)
        external
        onlyOwner()
    {
        isAddressExemptFromBNBRewards[removeAddress] = false;
    }

    function setMinimumBNBInContractToReward(uint256 newBNBMinimum)
        external
        onlyOwner()
    {
        bnbMinimumAmountInContractToReward = newBNBMinimum;
    }

    function setBNBMaxUntilCharityCut(uint256 newBNBMaxUntilCharityCut)
        external
        onlyOwner()
    {
        bnbMaxUntilCharityCut = newBNBMaxUntilCharityCut;
    }

    function setMinimumAmountToHoldForRewards(
        uint256 newMinimumAmountToHoldForRewards
    ) external onlyOwner() {
        minimumAmountToHoldForRewards = newMinimumAmountToHoldForRewards;
    }

    function setMaxNumOfTransersToDoForReward(uint256 newMaxNumOfTransfers)
        external
        onlyOwner()
    {
        maxNumOfTransfersToDoForReward = newMaxNumOfTransfers;
    }

    function setRewardSystemEnabledOrDisabled(bool isRewardSystemEnabledNew)
        external
        onlyOwner()
    {
        isRewardSystemEnabled = isRewardSystemEnabledNew;
    }

    function setHoursForRewardTimer(uint256 newNoursToAddForReward)
        external
        onlyOwner()
    {
        hoursToAddForReward = newNoursToAddForReward;
    }

    function setNewPercentOfCharityCut(uint256 newPercentOfCharityCut)
        external
        onlyOwner()
    {
        percentOfCharityCut = newPercentOfCharityCut; // new charity wallet
    }

    function setNewCharityWallet(address newCharityWallet)
        external
        onlyOwner()
    {
        charityWallet = newCharityWallet; // new charity wallet
    }

    function viewCurrentBNBinContract() public view returns (uint256) {
        return address(this).balance;
    }

    function payableAddress(address addressToBePayable)
        private
        pure
        returns (address payable)
    {
        // gets the sender of the payable address
        address payable payableMsgSender = payable(address(addressToBePayable));
        return payableMsgSender;
    }

    function setRouterAddress(address newRouter) public onlyOwner() {
        IUniswapV2Router02 _newPancakeRouter = IUniswapV2Router02(newRouter);
        uniswapV2Pair = IUniswapV2Factory(_newPancakeRouter.factory())
        .createPair(address(this), _newPancakeRouter.WETH());
        uniswapV2Router = _newPancakeRouter;
    }

    function setPairAddress(address newPairAddress) public onlyOwner() {
        uniswapV2Pair = newPairAddress;
    }

    function _transferStandard(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 rFee,
            uint256 tTransferAmount,
            uint256 tFee,
            uint256 tBNB,
            uint256 tLiquidity
        ) = _getValues(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);
        _takeLiquidity(tLiquidity);
        _takeBNB(tBNB);
        _reflectFee(rFee, tFee);
        if (!_isExcludedFromFee[sender])
            emit Transfer(sender, address(this), tBNB);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    receive() external payable {} // Oh it's payable alright.
}
