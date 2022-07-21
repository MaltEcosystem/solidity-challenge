# Malt Take Home Test

This repo contains a realistic code challenge for Malt protocol using similar contracts to real protocol contracts. The core contract in the challenge is the `SwingTrader` which is a contract that can buy and sell Malt on a particular AMM Malt pair.

The `contracts` directory contains two important files:
1. **SwingTrader.sol** - This is a contract that is used to trade on a particular Malt AMM pair. Each Malt pair would gets it's own `SwingTrader`. Therefore this contract only has to deal with Malt and the single ERC20 it is paired with. The actual trade execution is done by another contract using the `IDexHandler` interface (The exact contract doesn't matter for this exercise). It is expected that the contract will be sent the ERC20 token by some other process in the protocol and then when another contract that has the `STABILIZER_NODE_ROLE` calls `buyMalt` it will use that ERC20 to purchase Malt and track it's cost basis. Later `sellMalt` can be called and it will sell the Malt and transfer some profit to a contract called `rewardThrottle`.
2. **Permissions.sol** - this is the main access control contract that the swing trader inherits from.

## Your Task
The goal of this challenge is to add the ability for an external user to be able to invest in the `SwingTrader` and receive another token in return that represents their share of the funds in the contract.

Here is a selection of user stories:
* The user should be able to provide the ERC20 token to the `SwingTrader` and receive another ERC20 "shares" token that represents their ownership of the `SwingTrader` capital.
* The user should then be able to convert their "Swing Trader Shares Tokens" back into the underlying assets (the ERC20 and Malt) according to their ownership.
  * The redemption of "Swing Trader Shares Tokens" should be a two stage processes that enforces a delay between initiating the redemption and actually receiving the underlying funds from it.

**Bonus points if you write a suite of tests for the new code**

## Notes
* The challenge is deliberately left somewhat ambigous to allow for your own creativity to play a role in the challenge. There is no one single correct way to achieve the goals of the challenge.
* You are free to make any choices you want. You can install new libraries, write new contracts, change existing contracts or whatever else you want to do to implement the required features.
* There are a few interfaces used by the `SwingTrader` that don't have implementations in this repo. For the sake of this challenge the implementation of these contracts is not important.
* If you write tests it may require you to write mocks for the interfaces that do no have implementation in this repo.
* Keep in mind that the `SwingTrader` can have a mixture of Malt and the other ERC20 token at any given point. This affects some calculations that may need to be done.
* `IMaltDataLab` interface provides methods that could be useful like `priceTarget` and `smoothedMaltPrice` both of which are denominated in the ERC20 token from the Malt AMM pool the `SwingTrader` is in charge of. This interface can be used with the assumption that the implementation exists and is correct.
* You will likely be asked to explain why certain decisions were made. So taking notes about your decisions throughout the processes could be helpful.
