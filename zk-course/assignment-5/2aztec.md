# AZTEC Note and various proofs
## the concept of AZTEC Note
Data structures for private UTXO system. is encoded as follows:

- `note.nonce` (plays a role in enabling the revocation of spending keys)
- `note.asset_id`
- `note.val`
- `note.secret(construct` a hiding Pedersen commitment to hide the note details)
- `note.owner.x`
- `note.owner.y`

The state of notes are managed by a Note Registry for any given asset.
The user’s balance of any AZTEC asset is made up of the sum of all of the valid notes their address owns in a given Note Registry.

[Note](https://miro.medium.com/max/1400/1*-zMKwxuGPUjA7SnyfSGsDw.png)

Note types:
- `UTXO notes` (default note type and currently used by the protocol)
- El-Gamal treasury notes(enable a single 'account' to have their balance represented by a single treasury note)

### used to privately represent various assets
ACE(AZTEC Cryptography Engine) has two primary functions:
- first to delegate the validation of proofs to specific validation contracts
- secondly to process state update instructions inside note registries that result from the successful validated proofs.
  
### ref
[Aztec Yellow Paper](https://hackmd.io/@aztec-network/ByzgNxBfd)
[Aztec specification](https://github.com/AztecProtocol/specification#b---interest-streaming-via-aztec-notes)
[An Introduction to AZTEC](https://medium.com/aztec-protocol/an-introduction-to-aztec-47c70e875dc7)
[AZTEC under the hood: range proofs](https://medium.com/aztec-protocol/aztec-under-the-hood-range-proofs-5fa6fe83a46)

## various proofs: range proofs, swap proofs, dividend proofs and join-split proofs
### range proofs
for join-split transaction, need to satisfy the balancing relationship required. But for elliptic curve, -1 is actually p-1, which is a huge number! 

a range proof check that every encrypted number that enters cryptosystem is many orders of magnitude less than p/2, then it’s never possible to ‘wrap’ around the modulus boundary and create ‘negative’ numbers.

lazy range proofs: lazy evaluation. Simply put, don’t bother doing something unless you have to, and only do it when you need to. 

perform a range proof, all we need to do is present a signature, and prove it was signed by y.
### swap proofs 
provides transfer instructions for two zkAsset confidential digital assets. There will be 2 entries inside proofOutputs, with each entry mapping to one of the two confidential assets - zkAsset A and zkAsset B.

zkAsset A and zkAsset B do not know this (the proof in question could have been added to ACE after the creation of these contracts), ACE does, and can act as the ultimate arbiter of whether a transfer instruction is valid or not.

### dividend proofs
validates that an AZTEC UTXO note is equal to a public percentage of a second AZTEC UTXO note. 
This proof is belongs to the UTILITY category, as in isolation it does not describe a balancing relationship.

The Dividend proof involves three AZTEC notes and two scalars za, zb. The scalars za, zb define a ratio and the proof proves the following:
`note[1].value * za = note[2].value * zb + note[3].value`
See [Dividend.sol](https://github.com/AztecProtocol/specification#dividendsol) for a detailed notes explanation

### join-split proofs
takes a series of inputNotes, to be removed from a note registry, and a series of outputNotes to be added to the note registry. In addition, an integer publicValue can be supplied - this specifies the number of ERC20 tokens to be converted into AZTEC note form or from AZTEC note form.

The amount of public 'value' being used in the join-split proof, kPublic, is defined as the kBar value of the last entry in the uint[6][] notes array. This value is traditionally empty (the last note does not have a kBar parameter) and the space is re-used to house kPublic.

See [JoinSplit.sol](https://github.com/AztecProtocol/specification#joinsplitsol) for a detailed notes explanation

# AZTEC private loan application
Users can deposit and borrow money by using Aztec Connect, saving gas compared to the main network, and with privacy by default.

Deposit ERC20 tokens into Aztec and initiate deposit or loan requests through ZK.Money. Aztec's ZKrollup will trade in bulk across users and interact with L1 contract through bridging contracts. 
Users will receive a cToken on Aztec or any assets they borrow that can be used in other DeFi protocols on Aztec. 

key steps for aztec products:
1) Generate note; 2) Management of secret keys; 3) Verify the Note

## loan application steps
- build proofData
```
const {
   proofData,
} = aztec.proof.mint.encodeMintTransaction({
        newTotalMinted: newTotalNote,
        oldTotalMinted: oldTotalNote,
        adjustedNotes: [loanNotionalNote],
        senderAddress: loanDappContract.address,
});
```
- register note: proof could be Mint to new Note
```
Loan(loanId).confidentialMint(MINT_PROOF, bytes(_proofData));
```
- Settlement ZkAsset(approval, build proof and transfer)
- Settlement loan processing(approval, build proof, transfer and update state)

## loan constract
`loan.sol`
```
pragma solidity >= 0.5.0 <0.7.0;
import "@aztec/protocol/contracts/ERC1724/ZkAssetMintable.sol";
import "@aztec/protocol/contracts/libs/NoteUtils.sol";
import "@aztec/protocol/contracts/interfaces/IZkAsset.sol";
import "./LoanUtilities.sol";

contract Loan is ZkAssetMintable {

  using SafeMath for uint256;
  using NoteUtils for bytes;
  using LoanUtilities for LoanUtilities.LoanVariables;
  LoanUtilities.LoanVariables public loanVariables;


  IZkAsset public settlementToken;
  // [0] interestRate
  // [1] interestPeriod
  // [2] duration
  // [3] settlementCurrencyId
  // [4] loanSettlementDate
  // [5] lastInterestPaymentDate address public borrower;
  address public lender;
  address public borrower;

  mapping(address => bytes) lenderApprovals;

  event LoanPayment(string paymentType, uint256 lastInterestPaymentDate);
  event LoanDefault();
  event LoanRepaid();

// note for loan: (owner,noteHash) 
  struct Note {
    address owner;
    bytes32 noteHash;
  }

  function _noteCoderToStruct(bytes memory note) internal pure returns (Note memory codedNote) {
      (address owner, bytes32 noteHash,) = note.extractNote();
      return Note(owner, noteHash );
  }

// constructor fill LoanVariables structure:
/*
    uint256 interestRate;
    uint256 interestPeriod;
    uint256 duration;
    uint256 loanSettlementDate;
    uint256 lastInterestPaymentDate;
    bytes32 notional;
    bytes32 currentInterestBalance;
    address borrower;
    address lender;
    address loanFactory;
    address aceAddress;
    IZkAsset settlementToken;
    address id;
*/
  constructor(
    bytes32 _notional,
    uint256[] memory _loanVariables,
    address _borrower,
    address _aceAddress,
    address _settlementCurrency
   ) public ZkAssetMintable(_aceAddress, address(0), 1, true, false) {
      loanVariables.loanFactory = msg.sender;
      loanVariables.notional = _notional;
      loanVariables.id = address(this);
      loanVariables.interestRate = _loanVariables[0];
      loanVariables.interestPeriod = _loanVariables[1];
      loanVariables.duration = _loanVariables[2];
      loanVariables.borrower = _borrower;
      borrower = _borrower;
      loanVariables.settlementToken = IZkAsset(_settlementCurrency);
      loanVariables.aceAddress = _aceAddress;
  }

  function requestAccess() public {
    lenderApprovals[msg.sender] = '0x';
  }

  function approveAccess(address _lender, bytes memory _sharedSecret) public {
    lenderApprovals[_lender] = _sharedSecret;
  }

  function settleLoan(
    bytes calldata _proofData,
    bytes32 _currentInterestBalance,
    address _lender
  ) external {
    // sender must be the loan dapp
    LoanUtilities.onlyLoanDapp(msg.sender, loanVariables.loanFactory);
    // ACE valid proof: calldata proof for loanVariables.aceAddress 
    LoanUtilities._processLoanSettlement(_proofData, loanVariables);
    // update loanVariables
    loanVariables.loanSettlementDate = block.timestamp;
    loanVariables.lastInterestPaymentDate = block.timestamp;
    loanVariables.currentInterestBalance = _currentInterestBalance;
    loanVariables.lender = _lender;
    lender = _lender;
  }
  
  // register note: proof could be Mint to new Note
  function confidentialMint(uint24 _proof, bytes calldata _proofData) external {
    LoanUtilities.onlyLoanDapp(msg.sender, loanVariables.loanFactory);
    require(msg.sender == owner, "only owner can call the confidentialMint() method");
    require(_proofData.length != 0, "proof invalid");
    // overide this function to change the mint method to msg.sender
    (bytes memory _proofOutputs) = ace.mint(_proof, _proofData, msg.sender);

    (, bytes memory newTotal, ,) = _proofOutputs.get(0).extractProofOutput();

    (, bytes memory mintedNotes, ,) = _proofOutputs.get(1).extractProofOutput();

    (,
    bytes32 noteHash,
    bytes memory metadata) = newTotal.extractNote();

    logOutputNotes(mintedNotes);
    // emit update minted event
    emit UpdateTotalMinted(noteHash, metadata);
  }

  // caculate withdraw interest
  // valid proof1 and get outPutNote together with proof2 and loanVariables
  // to update InterestNoteHash
  function withdrawInterest(
    bytes memory _proof1,
    bytes memory _proof2,
    uint256 _interestDurationToWithdraw
  ) public {
    (,bytes memory _proof1OutputNotes) = LoanUtilities._validateInterestProof(_proof1, _interestDurationToWithdraw, loanVariables);

    require(_interestDurationToWithdraw.add(loanVariables.lastInterestPaymentDate) < block.timestamp, ' withdraw is greater than accrued interest');

    (bytes32 newCurrentInterestNoteHash) = LoanUtilities._processInterestWithdrawal(_proof2, _proof1OutputNotes, loanVariables);

    loanVariables.currentInterestBalance = newCurrentInterestNoteHash;
    loanVariables.lastInterestPaymentDate = loanVariables.lastInterestPaymentDate.add(_interestDurationToWithdraw);

    emit LoanPayment('INTEREST', loanVariables.lastInterestPaymentDate);

  }

  function adjustInterestBalance(bytes memory _proofData) public {

    LoanUtilities.onlyBorrower(msg.sender,borrower);

    (bytes32 newCurrentInterestBalance) = LoanUtilities._processAdjustInterest(_proofData, loanVariables);
    loanVariables.currentInterestBalance = newCurrentInterestBalance;
  }
  
  // first must check interest proof
  // sender must be the borrower
  function repayLoan(
    bytes memory _proof1,
    bytes memory _proof2
  ) public {
    LoanUtilities.onlyBorrower(msg.sender, borrower);

    uint256 remainingInterestDuration = loanVariables.loanSettlementDate.add(loanVariables.duration).sub(loanVariables.lastInterestPaymentDate);

    (,bytes memory _proof1OutputNotes) = LoanUtilities._validateInterestProof(_proof1, remainingInterestDuration, loanVariables);

    require(loanVariables.loanSettlementDate.add(loanVariables.duration) < block.timestamp, 'loan has not matured');


    LoanUtilities._processLoanRepayment(
      _proof2,
      _proof1OutputNotes,
      loanVariables
    );

    emit LoanRepaid();
  }

  function markLoanAsDefault(bytes memory _proof1, bytes memory _proof2, uint256 _interestDurationToWithdraw) public {
    require(_interestDurationToWithdraw.add(loanVariables.lastInterestPaymentDate) < block.timestamp, 'withdraw is greater than accrued interest');
    LoanUtilities._validateDefaultProofs(_proof1, _proof2, _interestDurationToWithdraw, loanVariables);
    emit LoanDefault();
  }
}
```
`LoanDapp.sol`
```
pragma solidity >= 0.5.0 <0.7.0;

import "@aztec/protocol/contracts/interfaces/IAZTEC.sol";
import "@aztec/protocol/contracts/libs/NoteUtils.sol";
import "@aztec/protocol/contracts/ERC1724/ZkAsset.sol";
import "./ZKERC20/ZKERC20.sol";
import "./Loan.sol";

// inheritance interface constract IAZTEC
// interaction with user for approve/loan/settlement
// define dapp event types: SettlementCurrencyAdded/LoanApprovedForSettlement
// LoanCreated/ViewRequestCreated/ViewRequestApproved
contract LoanDapp is IAZTEC {
  using NoteUtils for bytes;

  event SettlementCurrencyAdded(
    uint id,
    address settlementAddress
  );

  event LoanApprovedForSettlement(
    address loanId
  );

  event LoanCreated(
    address id,
    address borrower,
    bytes32 notional,
    string borrowerPublicKey,
    uint256[] loanVariables,
    uint createdAt
  );

  event ViewRequestCreated(
    address loanId,
    address lender,
    string lenderPublicKey
  );

  event ViewRequestApproved(
    uint accessId,
    address loanId,
    address user,
    string sharedSecret
  );

  event NoteAccessApproved(
    uint accessId,
    bytes32 note,
    address user,
    string sharedSecret
  );

  address owner = msg.sender;
  address aceAddress;
  address[] public loans;
  mapping(uint => address) public settlementCurrencies;

  uint24 MINT_PRO0F = 66049;
  uint24 BILATERAL_SWAP_PROOF = 65794;

  modifier onlyOwner() {
    require(msg.sender == owner);
    _;
  }

  modifier onlyBorrower(address _loanAddress) {
    Loan loanContract = Loan(_loanAddress);
    require(msg.sender == loanContract.borrower());
    _;
  }

  constructor(address _aceAddress) public {
    aceAddress = _aceAddress;
  }

  function _getCurrencyContract(uint _settlementCurrencyId) internal view returns (address) {
    require(settlementCurrencies[_settlementCurrencyId] != address(0), 'Settlement Currency is not defined');
    return settlementCurrencies[_settlementCurrencyId];
  }

  function _generateAccessId(bytes32 _note, address _user) internal pure returns (uint) {
    return uint(keccak256(abi.encodePacked(_note, _user)));
  }

  function _approveNoteAccess(
    bytes32 _note,
    address _userAddress,
    string memory _sharedSecret
  ) internal {
    uint accessId = _generateAccessId(_note, _userAddress);
    emit NoteAccessApproved(
      accessId,
      _note,
      _userAddress,
      _sharedSecret
    );
  }

  function _createLoan(
    bytes32 _notional,
    uint256[] memory _loanVariables,
    bytes memory _proofData
  ) private returns (address) {
    address loanCurrency = _getCurrencyContract(_loanVariables[3]);

    Loan newLoan = new Loan(
      _notional,
      _loanVariables,
      msg.sender,
      aceAddress,
      loanCurrency
    );

    loans.push(address(newLoan));
    Loan loanContract = Loan(address(newLoan));

    loanContract.setProofs(1, uint256(-1));
    loanContract.confidentialMint(MINT_PROOF, bytes(_proofData));

    return address(newLoan);
  }

  // settlement user's currency balance 
  // last emit settlement event 
  function addSettlementCurrency(uint _id, address _address) external onlyOwner {
    settlementCurrencies[_id] = _address;
    emit SettlementCurrencyAdded(_id, _address);
  }

  // loan request config, set loan contract proof and set proof could be Mint to new Note
  function createLoan(
    bytes32 _notional,
    string calldata _viewingKey,
    string calldata _borrowerPublicKey,
    uint256[] calldata _loanVariables,
    // [0] interestRate
    // [1] interestPeriod
    // [2] loanDuration
    // [3] settlementCurrencyId
    bytes calldata _proofData
  ) external {
    address loanId = _createLoan(
      _notional,
      _loanVariables,
      _proofData
    );

    emit LoanCreated(
      loanId,
      msg.sender,
      _notional,
      _borrowerPublicKey,
      _loanVariables,
      block.timestamp
    );

    _approveNoteAccess(
      _notional,
      msg.sender,
      _viewingKey
    );
  }

 // approve contract interaction：loan requet with user note's hash+signature additional with current id
  function approveLoanNotional(
    bytes32 _noteHash,
    bytes memory _signature,
    address _loanId
  ) public {
    Loan loanContract = Loan(_loanId);
    loanContract.confidentialApprove(_noteHash, _loanId, true, _signature);
    emit LoanApprovedForSettlement(_loanId);
  }

  function submitViewRequest(address _loanId, string calldata _lenderPublicKey) external {
    emit ViewRequestCreated(
      _loanId,
      msg.sender,
      _lenderPublicKey
    );
  }

  function approveViewRequest(
    address _loanId,
    address _lender,
    bytes32 _notionalNote,
    string calldata _sharedSecret
  ) external onlyBorrower(_loanId) {
    uint accessId = _generateAccessId(_notionalNote, _lender);

    emit ViewRequestApproved(
      accessId,
      _loanId,
      _lender,
      _sharedSecret
    );
  }

  event SettlementSuccesfull(
    address indexed from,
    address indexed to,
    address loanId,
    uint256 timestamp
  );

  struct LoanPayment {
    address from;
    address to;
    bytes notional;
  }

  mapping(uint => mapping(uint => LoanPayment)) public loanPayments;

  function settleInitialBalance(
    address _loanId,
    bytes calldata _proofData,
    bytes32 _currentInterestBalance
  ) external {
    Loan loanContract = Loan(_loanId);
    loanContract.settleLoan(_proofData, _currentInterestBalance, msg.sender);
    emit SettlementSuccesfull(
      msg.sender,
      loanContract.borrower(),
      _loanId,
      block.timestamp
    );
  }

  // approve contract interaction：with user note address
  // need key and secret
  function approveNoteAccess(
    bytes32 _note,
    string calldata _viewingKey,
    string calldata _sharedSecret,
    address _sharedWith
  ) external {
    if (bytes(_viewingKey).length != 0) {
      _approveNoteAccess(
        _note,
        msg.sender,
        _viewingKey
      );
    }

    if (bytes(_sharedSecret).length != 0) {
      _approveNoteAccess(
        _note,
        _sharedWith,
        _sharedSecret
      );
    }
  }
}
```