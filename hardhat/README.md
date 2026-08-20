# Ritual Predict — contracts

The `RitualPredict` market contract, its tests, and the deployment scripts.
Full architecture and the workshop runbook live in [../README.md](../README.md).

## Layout

```
contracts/
  RitualPredict.sol          the market: creation, betting, autonomous resolution, payouts
  RitualPredict.t.sol        Solidity unit tests
  ritual/RitualChain.sol     canonical Ritual addresses + system contract interfaces
  mocks/RitualMocks.sol      test-only stand-ins for the precompiles and system contracts
test/
  RitualPredict.e2e.ts       end-to-end walkthroughs of the workshop flow
scripts/
  block-time.ts              measure the chain's current block time
  deploy.ts                  deploy + prepay execution fees
  fund.ts                    top up the prepaid execution balance
  status.ts                  live state of every market
  create-demo-market.ts      create the preset market from the CLI
  export-abi.ts              copy the compiled ABI into the frontend
```

## Commands

```bash
cp .env.example .env                            # RITUAL_PRIVATE_KEY, funded from the faucet

npx hardhat test                                # 33 Solidity + 2 TypeScript tests
npx hardhat test solidity                       # Solidity only
npx hardhat build                               # compile

npx hardhat run scripts/block-time.ts           # measure block time
npx hardhat run scripts/deploy.ts               # deploy to Ritual Chain
PREDICT_ADDRESS=0x... npx hardhat run scripts/status.ts
PREDICT_ADDRESS=0x... npx hardhat run scripts/fund.ts
```

Tests run entirely against mocks — `vm.etch` puts the mock runtime code at the canonical Ritual
addresses — so no network access or funded account is needed.

## Local Build Notes

I forked the workshop repository and verified the project locally on Windows using Node.js, pnpm, and Hardhat.

* Installed the project dependencies successfully.
* Compiled the Solidity contracts successfully with solc 0.8.28.
* Found that the starter repository still included an obsolete `Counter.ts` test while the `Counter` contract was no longer present.
* Removed the obsolete Counter test and committed the fix to my fork.
* Ran the available Hardhat test commands locally.
* The current fork does not include the RitualPredict test files referenced in the README, so the Solidity test command currently reports 0 passing tests instead of the documented test count.

This local debugging process helped me understand the project structure, Hardhat workflow, and the current state of the workshop starter repository.
