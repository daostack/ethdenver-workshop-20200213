# ETHDENVER WORKSHOP 
# Let's add a Generic Plugin to DAOstack

----

# Hi

I'm Jelle Gerbrandy and I work for DAOstack

# While I presents some context, you install your environment

You already have npm and git. And docker-compose :-)

```
git clone git@github.com:daostack/alchemy.git
cd alchemy
```
Install alchemy:
```
npm install
npm run start
```
and while you wait, in another terminal:
```
docker-compose up graph-node
```

# Intro:

* DAOstack Stack:
  - Contracts on Ethereum  ([Arc](https://github.com/daostack/arc) and [Infra](https://github.com/daostack/infra))
  - [ArcGraph](https://github.com/daostack/subgraph) [indexing for fast access] 
  - [Alchemy](https://github.com/daostack/alchemy)

* Architecture
 
 [DAO (owns and controls assets, has members with reputation)] 
 <--> 
 [one or more "plugins"] 
 <--> 
 [The world [other smart contracts + ethereum accounts (i.e. ppl)]


# Plugins

- Visit http://127.0.0.1:8000
- Find the DutchX DAO
- Exercise: Create a proposal in it
  - [use account with private key `4F3EDF983AC636A65A842CE7C78D9AA706D3B113BCE9C46F30D7D21715B23B1D`]
  - [use Metamask]
  - [Bugs: please report them [here](https://github.com/daostack/alchemy/issues)]

Examples of plugins:

- "Funding and Voting power" --> for (decisions about) distributing assets and reputation
- "Plugin manager" -> for (descisions about) adding and removing plugins from the DAO
- "Generic Scheme" -> for (decisions about) calling any method on any contract
    - like dutchx, ens schemes, etc


# Let's get to work


Your DAO holds Ether that it does not want to spend just yet. 
So lets put this money to work, and add it to uniswap: provide some liquidity to our DeFi friends and earn some fees in the process...


We will create a plugin for [Uniswap](https://uniswap.exchange/add-liquidity) to add and remove liquidity.


## Step 1: Specs!

OurDAO has ETH and wants to add that to the 
[ETH/sETH liquidity pool](https://uniswap.info/token/0x5e74c9036fb86bd7ecdcb084a0673efc32ea31cb).

To do that, you will want to:

1. Buy some sETH with your ETH
1. Allow the uniswap contract to spend your sEth and ETH
1. Add the ETH and sETH to the uniswap liquidity pool. 
1. Remove it form the liquidity pool

And we will want to do this with a UI like this one:  

## Step 2: Register a new GenericScheme in your DAO 

...that is allowed to call the uniswap contract.

**[WE will not do this now]**


1. Where is the contract?  [Here at 0xe9cf7887b93150d4f2da7dfc6d502b216438f244](https://etherscan.io/address/0xe9cf7887b93150d4f2da7dfc6d502b216438f244/) 
2. Create a proposal to register a generic scheme that calls this contract
3. Get is accepted and executed

[we skip this step bc we are on a local dev env and it does not matter as we are only creating the interface on the testnet, instead we will use the DutchX interface as a template]

## Step 3: Create forms to create proposals

1. Find the dutchregistry json file: `src/genericSchemeRegistry/schemes/DutchX.json` a
1. Copy it to a new file called `uniswap.json`
1. Remove the address under "private" in the ducthx.json file so there is not confusion
1. Edit `GenericSchemeRegistry/index.ts`, import your new scheme, and add it to the KNOWN_SCHEMES

We'll use the new  file as a template
1. Keep your browser open [here](http://127.0.0.1:3000/dao/0xac48330d4b96fc74c76ec2ac33a877e9638c1baf/scheme/0xf2db935504f2957538fb63082b2db962321a7938e5079bba145338546edee712/proposals/create/)
1. Play around: change the title, edit the acitons
1. Create the input form for each of the actions. You'll need to find the ABI for each of the actions, which you can find here: 


## Step 4: Show your proposal 

If you are not happy with the default way of how the content of your proposal is represented, you can create a component for it.

Use [ProposalSummaryDutchX.tsx](https://github.com/daostack/alchemy/blob/dev/src/components/Proposal/ProposalSummary/ProposalSummaryDutchX.tsx) as a template, and add a 
link to line on [ProposalSummaryKnownGenericScheme.tsx](https://github.com/daostack/alchemy/blob/dev/src/components/Proposal/ProposalSummary/ProposalKnownGenericScheme.tsx)

# That's it for now

## Ideas to hack on:

 Make alchemy properly pluggable for Generic Schemes:

 A: Make the "registry"  not part of the alchemy code, but from a separate package
 B: Make is so `ProposalSummaryDutchX.tsx` can be removed - instead, it will be controlled by the JSON file
 C: We have quickly hacked a number of form input widgets (commanded from the ABI + some extra insturctions in the JSON file). Extend these. Or use, choose and integrate an library




