[//]: # (GENERATED DOC.)
[//]: # (DO NOT EDIT MANUALLY!!!)

# x/deterministicgas

## Intro

Coreum uses a deterministic gas model for its transactions. Meaning that given a transaction type (e.g
`/coreum.asset.ft.v1.MsgIssueGasPrice`) one can know how much gas will be used beforehand, and this amount is fixed if some
preconditions are met. Of course this deterministic gas does not apply to the type of transactions that have a
complicated, nondeterministic execution path (e.g `/cosmwasm.wasm.v1.MsgExecuteContract`). We provide tables with all
[deterministic gas](#deterministic-messages) & [nondeterministic gas](#nondeterministic-messages) for all our types.

## Formula

Here is formula for the transaction

`
Gas = FixedGas + Sum(Gas for each message) + GasForExtraBytes + GasForExtraSignatures
`

If message type is deterministic, then the value is looked up from the table, if it is non-deterministic, then the
required gas is determined after the execution.

`
GasForExtraBytes = max(0, TxByteSize-FreeBytes) * TxSizeCostPerByte
`

`
GasForExtraSignatures = max(0, NumOfSigs-FreeSigs) * SigVerifyCost
`

Currently, we have values for the above variables as follows:

- `FixedGas`: 65000
- `SigVerifyCost`: 1000
- `TxSizeCostPerByte`: 10
- `FreeSignatures`: 1
- `FreeBytes`: 2048

As an example if the transaction has 1 signature on it and is below
2048 bytes, the user will not pay anything extra, and if one of those values exceed those limits, the user will pay for
the extra resources.

### Full examples

#### Example 1
Let's say we have a transaction with 1 messages of type
`/cosmos.bank.v1beta1.MsgSend` containing single coin inside, also there is a single
signatures and the tx size is 1000 bytes, total will be:

`
TotalGas = 65000 +  1 * 50000 + 1 * 1000 + max(0, 1000-2048) * 10
`

#### Example 2
Let's say we have a transaction with 2 messages of type
`/coreum.asset.ft.v1.MsgIssueGasPrice` inside, also there are 2
signatures and the tx size is 2050 bytes, total will be:

`
TotalGas = 65000 + 2 * 70000 + 2 * 1000 + max(0, 2050-2048) * 10
`

## Gas Tables

### Deterministic messages

| Message Type | Gas |
|--------------|-----|
| `/cosmos.authz.v1beta1.MsgExec`                                        | [special case](#special-cases) |
| `/cosmos.bank.v1beta1.MsgMultiSend`                                    | [special case](#special-cases) |
| `/cosmos.bank.v1beta1.MsgSend`                                         | [special case](#special-cases) |
| `/coreum.asset.ft.v1.MsgBurn`                                          | 35000                          |
| `/coreum.asset.ft.v1.MsgFreeze`                                        | 8500                           |
| `/coreum.asset.ft.v1.MsgGloballyFreeze`                                | 5000                           |
| `/coreum.asset.ft.v1.MsgGloballyUnfreeze`                              | 5000                           |
| `/coreum.asset.ft.v1.MsgIssue`                                         | 70000                          |
| `/coreum.asset.ft.v1.MsgMint`                                          | 31000                          |
| `/coreum.asset.ft.v1.MsgSetFrozen`                                     | 8500                           |
| `/coreum.asset.ft.v1.MsgSetWhitelistedLimit`                           | 9000                           |
| `/coreum.asset.ft.v1.MsgUnfreeze`                                      | 8500                           |
| `/coreum.asset.ft.v1.MsgUpgradeTokenV1`                                | 25000                          |
| `/coreum.asset.nft.v1.MsgAddToClassWhitelist`                          | 7000                           |
| `/coreum.asset.nft.v1.MsgAddToWhitelist`                               | 7000                           |
| `/coreum.asset.nft.v1.MsgBurn`                                         | 26000                          |
| `/coreum.asset.nft.v1.MsgFreeze`                                       | 8000                           |
| `/coreum.asset.nft.v1.MsgIssueClass`                                   | 16000                          |
| `/coreum.asset.nft.v1.MsgMint`                                         | 39000                          |
| `/coreum.asset.nft.v1.MsgRemoveFromClassWhitelist`                     | 3500                           |
| `/coreum.asset.nft.v1.MsgRemoveFromWhitelist`                          | 3500                           |
| `/coreum.asset.nft.v1.MsgUnfreeze`                                     | 5000                           |
| `/coreum.nft.v1beta1.MsgSend`                                          | 25000                          |
| `/cosmos.authz.v1beta1.MsgGrant`                                       | 28000                          |
| `/cosmos.authz.v1beta1.MsgRevoke`                                      | 8000                           |
| `/cosmos.distribution.v1beta1.MsgFundCommunityPool`                    | 17000                          |
| `/cosmos.distribution.v1beta1.MsgSetWithdrawAddress`                   | 5000                           |
| `/cosmos.distribution.v1beta1.MsgWithdrawDelegatorReward`              | 79000                          |
| `/cosmos.distribution.v1beta1.MsgWithdrawValidatorCommission`          | 22000                          |
| `/cosmos.feegrant.v1beta1.MsgGrantAllowance`                           | 11000                          |
| `/cosmos.feegrant.v1beta1.MsgRevokeAllowance`                          | 2500                           |
| `/cosmos.gov.v1.MsgDeposit`                                            | 52000                          |
| `/cosmos.gov.v1.MsgVote`                                               | 6000                           |
| `/cosmos.gov.v1.MsgVoteWeighted`                                       | 6500                           |
| `/cosmos.gov.v1beta1.MsgDeposit`                                       | 85000                          |
| `/cosmos.gov.v1beta1.MsgVote`                                          | 6000                           |
| `/cosmos.gov.v1beta1.MsgVoteWeighted`                                  | 9000                           |
| `/cosmos.nft.v1beta1.MsgSend`                                          | 25000                          |
| `/cosmos.slashing.v1beta1.MsgUnjail`                                   | 25000                          |
| `/cosmos.staking.v1beta1.MsgBeginRedelegate`                           | 157000                         |
| `/cosmos.staking.v1beta1.MsgCreateValidator`                           | 117000                         |
| `/cosmos.staking.v1beta1.MsgDelegate`                                  | 83000                          |
| `/cosmos.staking.v1beta1.MsgEditValidator`                             | 13000                          |
| `/cosmos.staking.v1beta1.MsgUndelegate`                                | 112000                         |
| `/cosmos.vesting.v1beta1.MsgCreateVestingAccount`                      | 30000                          |
| `/cosmwasm.wasm.v1.MsgClearAdmin`                                      | 6500                           |
| `/cosmwasm.wasm.v1.MsgUpdateAdmin`                                     | 8000                           |
| `/ibc.applications.transfer.v1.MsgTransfer`                            | 54000                          |

#### Special Cases

There are some special cases when custom logic is applied for deterministic gas calculation.
Real examples of special case tests could be found [here](https://github.com/CoreumFoundation/coreum/blob/master/x/deterministicgas/config_test.go#L168)

##### `/cosmos.bank.v1beta1.MsgSend`

`DeterministicGasForMsg = bankSendPerCoinGas * NumberOfCoins`

`bankSendPerCoinGas` is currently equal to `50000`.

##### `/cosmos.bank.v1beta1.MsgMultiSend`

`DeterministicGasForMsg = bankMultiSendPerOperationGas * (NumberOfInputs + NumberOfOutputs)`

`bankMultiSendPerOperationGas` is currently equal to `35000`.

##### `/cosmos.authz.v1beta1.MsgExec`

`DeterministicGasForMsg = authzMsgExecOverhead + Sum(DeterministicGas(ChildMsg))`

`authzMsgExecOverhead` is currently equal to `2000`.

### Nondeterministic messages

| Message Type |
|--------------|
| `/cosmos.auth.v1beta1.MsgUpdateParams`                                 |
| `/cosmos.bank.v1beta1.MsgSetSendEnabled`                               |
| `/cosmos.bank.v1beta1.MsgUpdateParams`                                 |
| `/cosmos.consensus.v1.MsgUpdateParams`                                 |
| `/cosmos.crisis.v1beta1.MsgUpdateParams`                               |
| `/cosmos.crisis.v1beta1.MsgVerifyInvariant`                            |
| `/cosmos.distribution.v1beta1.MsgCommunityPoolSpend`                   |
| `/cosmos.distribution.v1beta1.MsgUpdateParams`                         |
| `/cosmos.evidence.v1beta1.MsgSubmitEvidence`                           |
| `/cosmos.gov.v1.MsgExecLegacyContent`                                  |
| `/cosmos.gov.v1.MsgSubmitProposal`                                     |
| `/cosmos.gov.v1.MsgUpdateParams`                                       |
| `/cosmos.gov.v1beta1.MsgSubmitProposal`                                |
| `/cosmos.mint.v1beta1.MsgUpdateParams`                                 |
| `/cosmos.slashing.v1beta1.MsgUpdateParams`                             |
| `/cosmos.staking.v1beta1.MsgCancelUnbondingDelegation`                 |
| `/cosmos.staking.v1beta1.MsgUpdateParams`                              |
| `/cosmos.upgrade.v1beta1.MsgCancelUpgrade`                             |
| `/cosmos.upgrade.v1beta1.MsgSoftwareUpgrade`                           |
| `/cosmos.vesting.v1beta1.MsgCreatePeriodicVestingAccount`              |
| `/cosmos.vesting.v1beta1.MsgCreatePermanentLockedAccount`              |
| `/cosmwasm.wasm.v1.MsgExecuteContract`                                 |
| `/cosmwasm.wasm.v1.MsgIBCCloseChannel`                                 |
| `/cosmwasm.wasm.v1.MsgIBCSend`                                         |
| `/cosmwasm.wasm.v1.MsgInstantiateContract`                             |
| `/cosmwasm.wasm.v1.MsgInstantiateContract2`                            |
| `/cosmwasm.wasm.v1.MsgMigrateContract`                                 |
| `/cosmwasm.wasm.v1.MsgPinCodes`                                        |
| `/cosmwasm.wasm.v1.MsgStoreAndInstantiateContract`                     |
| `/cosmwasm.wasm.v1.MsgStoreCode`                                       |
| `/cosmwasm.wasm.v1.MsgSudoContract`                                    |
| `/cosmwasm.wasm.v1.MsgUnpinCodes`                                      |
| `/cosmwasm.wasm.v1.MsgUpdateInstantiateConfig`                         |
| `/cosmwasm.wasm.v1.MsgUpdateParams`                                    |
| `/ibc.core.channel.v1.MsgAcknowledgement`                              |
| `/ibc.core.channel.v1.MsgChannelCloseConfirm`                          |
| `/ibc.core.channel.v1.MsgChannelCloseInit`                             |
| `/ibc.core.channel.v1.MsgChannelOpenAck`                               |
| `/ibc.core.channel.v1.MsgChannelOpenConfirm`                           |
| `/ibc.core.channel.v1.MsgChannelOpenInit`                              |
| `/ibc.core.channel.v1.MsgChannelOpenTry`                               |
| `/ibc.core.channel.v1.MsgRecvPacket`                                   |
| `/ibc.core.channel.v1.MsgTimeout`                                      |
| `/ibc.core.channel.v1.MsgTimeoutOnClose`                               |
| `/ibc.core.client.v1.MsgCreateClient`                                  |
| `/ibc.core.client.v1.MsgSubmitMisbehaviour`                            |
| `/ibc.core.client.v1.MsgUpdateClient`                                  |
| `/ibc.core.client.v1.MsgUpgradeClient`                                 |
| `/ibc.core.connection.v1.MsgConnectionOpenAck`                         |
| `/ibc.core.connection.v1.MsgConnectionOpenConfirm`                     |
| `/ibc.core.connection.v1.MsgConnectionOpenInit`                        |
| `/ibc.core.connection.v1.MsgConnectionOpenTry`                         |

[//]: # (GENERATED DOC.)
[//]: # (DO NOT EDIT MANUALLY!!!)
