---
title: wtf-lib API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for wtf-lib
---

# Introduction

Welcome to the docs for Holonym. 
Here you can find docs for 

- wtf-lib, a JavaScript library that allows developers to query WTF protocol smart contracts

- WTF smart contracts

# wtf-lib
## Configuration

> Import wtf-lib (CommonJS)

```javascript
const wtf = require('wtf-lib'); 
```

> Import wtf-lib (ESM)

```javascript
import wtf from 'wtf-lib';
```


> Tell wtf to use the Gnosis Chain JSON RPC provider exposed at https://rpc.gnosischain.com/ for all calls to WTF smart contracts on Gnosis Chain.

```javascript
wtf.setProviderURL({gnosis: 'https://rpc.gnosischain.com/'})
```

Import and set the URL(s) of your JSON RPC provider(s) (e.g., to your Pocket, Infura, or Moralis node). It is recommended that projects call this function to configure wtf-lib to use their provider and do not rely on wtf-lib default node. It is also best practice to use a custom provider on Pocket, Infura, Alchemy, etc. instead of the networks default RPC because these custom RPCs are faster.

<aside class="notice">
The default provider links to Ropsten testnet. If you are on a different network, you should use a custom provider. You will have best performance with a custom RPC provider rather than a network's default RPC. 
</aside>


## getHolo(address)

> Get a user's holo (i.e., their name, bio, and all their credentials).

```javascript
const userHolo = await wtf.getHolo('0xdbd6b2c02338919edaa192f5b60f5e5840a50079')
```

> Returns

```json
{
  "gnosis": {
    "name": "Jerry",
    "bio": "The gerbil (the real one) 🐹",
    "twitter": "",
    "github": "",
    "discord": "",
    "orcid": "",
    "google": "thejerrygerbil@gmail.com"
  },
  "ethereum": {
      ...
  }
}
```

This function returns the user's name, bio, and all their credentials, given their address. 

It organizes the user's holo by blockchain network. A user can use the same address to register different credentials on different blockchains. By organizing a user's holo by network, `getHolo` allows projects to use holos from only certain networks and to resolve any contradictions that might arise with using multiple profiles.


Parameter | Description
--------- | -----------
address | A user's crypto address.


## credentialsForAddress(address, service)

> Get a user's gmail.

```javascript
const userGmail = await wtf.credentialsForAddress('0xdbd6b2c02338919edaa192f5b60f5e5840a50079', 'google')
```

> Returns

```javascript
'thejerrygerbil@gmail.com'
```

This function returns the user's credentials (e.g., their ORCID ID or gmail), given the user's crypto address. 


Parameter | Description
--------- | -----------
address | A user's crypto address.
service | The service that issued the credentials (e.g., ORCID or Google).

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->


## addressForCredentials(credentials, service)

> Get a user's crypto address.

```javascript
const userAddress = await wtf.addressForCredentials('thejerrygerbil@gmail.com', 'google')
```

> Returns

```javascript
'0xdbd6b2c02338919edaa192f5b60f5e5840a50079'
```

This function returns the user's crypto address, given their credentials (e.g., their ORCID ID or gmail). 


Parameter | Description
--------- | -----------
credentials | A user's credentials.
service | The service that issued the credentials (e.g., ORCID or Google).


## nameForAddress(address)

> Get a user's name.

```javascript
const userName = await wtf.nameForAddress('0xdbd6b2c02338919edaa192f5b60f5e5840a50079')
```

> Returns

```javascript
'Jerry'
```

This function returns the user's name, given their address.


Parameter | Description
--------- | -----------
address | A user's crypto address.

## bioForAddress(address)


> Get a user's bio/description.

```javascript
const userBio = await wtf.bioForAddress('0xdbd6b2c02338919edaa192f5b60f5e5840a50079')
```

> Returns

```javascript
'The gerbil (the real one) 🐹'
```

This function returns the user's bio/description, given their address.


Parameter | Description
--------- | -----------
address | A user's crypto address.


## getContractAddresses()

> Get the addresses of all deployed WTF contracts.

```javascript
const wtfContractAddrs = await wtf.getContractAddresses()
```

> Returns

```javascript
{
  "production": {
    "IdentityAggregator": {
      "gnosis": "0x..."
    },
    "WTFBios": {
      "gnosis": "0x..."
    },
    "VerifyJWT": {
      "gnosis": {
        "orcid": "0x...",
        "google": "0x...",
        "twitter": "0x...",
        "github" : "0x...",
        "discord" : "0x..."
      }
    }
  }
}
```

You can use this function if you would like to call the WTF contracts directly from your JavaScript. This function returns a JSON object containing all the contract addresses for the different WTF contracts. 

## getContractABIs()

> Get the interfaces of all deployed WTF contracts.

```javascript
const wtfContractInterfaces = await wtf.getContractABIs()
```

> Returns

```javascript
{
    "IdentityAggregator": [
        "event AddSupportForContract(string)",
        ...
        "function getAllAccounts(address) view returns (bytes[], string, string)",
        ...
    ]
    "WTFBios": [
        "event RemoveUserNameAndBio(address)",
        ...
        "function bioForAddress(address) view returns (string)",
        ...
    ]
    "VerifyJWT": [
        ...
        "function credsForAddress(address) view returns (bytes)",
        ...
    ]
}
```

You can use this function if you would like to call the WTF contracts directly from your JavaScript. This function returns a JSON object containing all the contract addresses for the different WTF contracts. 


# WTF smart contracts
## Introduction
The WTF protocol consists of 3 types of smart contracts:

- VerifyJWT (verifies JWTs and stores user's credentials (e.g., their ORCID))

- WTFBios (allows users to set the name and bio associated with their Holo)

- IdentityAggregator (aggregates all profile data for each Holonym user)

For any blockchain network to which WTF contracts have been deployed, there will be multiple VerifyJWT contracts but only one WTFBios contract and one IdentityAggregator contract. There is a VerifyJWT contract for Twitter, one for ORCID, etc.

All credentials (e.g., a user's Twitter handle) are stored as bytes. You can convert bytes to string in JavaScript with the following line: `Buffer.from(creds.replace('0x',''), 'hex').toString()`.

WTF contracts are currently deployed on Gnosis Chain. See the [contract addresses section](#contract-addresses) for the full list of contract addresses.

Each contract exposes functions to query user data.

Note: The following code snippets are Solidity, not JavaScript.

## IdentityAggregator.getAllAccounts(address user)

> Get all accounts associated with this user's crypto address. 

```solidity
IdentityAggregator idAggregator = IdentityAggregator(0x4278b0B8aC44dc61579d5Ec3F01D8EA44873b079);
(bytes[] memory creds, string memory name, string memory bio) = idAggregator.getAllAccounts(0xdbd6b2c02338919edaa192f5b60f5e5840a50079)
```

> Return values

```solidity
creds == [0x..., 0x...]
name == 'Jerry'
bio == 'The gerbil (the real one) 🐹'
```

Get all accounts associated with this user's crypto address. The `getHolo` function in wtf-lib uses `getAllAccounts` under the hood. We expect creds to be converted to strings in the front end.

Parameter | Description
--------- | -----------
user | A user's crypto address.

## VerifyJWT.credsForAddress(address user)

> Get the creds linked to this user's crypto address on this specific VerifyJWT contract.

```solidity
VerifyJWT vjwt = VerifyJWT(0x97A2FAf052058b86a52A07F730EF8a16aD9aFcFB);
bytes memory creds = vjwt.credsForAddress(0xdbd6b2c02338919edaa192f5b60f5e5840a50079)
```

> Return value

```solidity
creds == 0x...
```

Get a user's credentials (e.g., their Twitter handle), given their address.

Parameter | Description
--------- | -----------
address | A user's crypto address.


## VerifyJWT.addressForCreds(bytes creds)

> Get the address linked to theses specific credentials on this specific VerifyJWT contract.

```solidity
VerifyJWT vjwt = VerifyJWT(0x97A2FAf052058b86a52A07F730EF8a16aD9aFcFB);
address user = vjwt.addressForCreds(0x...)
```

> Return value

```solidity
user == 0xdbd6b2c02338919edaa192f5b60f5e5840a50079
```

Get a user's address, given their credentials (e.g., their Twitter handle).

Parameter | Description
--------- | -----------
creds | A user's credentials represented as bytes.


## WTFBios.bioForAddress(address user)

> Get a user's Holonym bio

```solidity
WTFBios wtfBios = WTFBios(0x99708c7c7d4895cFF1C81d0Ff96152b5AEcc3E78);
string memory bio = wtfBios.bioForAddress(0xdbd6b2c02338919edaa192f5b60f5e5840a50079)
```

> Return value

```solidity
bio == 'The gerbil (the real one) 🐹'
```

Get a user's Holonym bio, given their address.

Parameter | Description
--------- | -----------
user | A user's crypto address.

## WTFBios.nameForAddress(address user)

> Get a user's Holonym name

```solidity
WTFBios wtfBios = WTFBios(0x99708c7c7d4895cFF1C81d0Ff96152b5AEcc3E78);
string memory name = wtfBios.nameForAddress(0xdbd6b2c02338919edaa192f5b60f5e5840a50079)
```

> Return value

```solidity
name == 'Jerry'
```

Get a user's name, given their address.

Parameter | Description
--------- | -----------
user | A user's crypto address.

## Contract Addresses
Contract addresses (on Gnosis Chain):

IdentityAggregator: 0x4278b0B8aC44dc61579d5Ec3F01D8EA44873b079

WTFBios: 0x99708c7c7d4895cFF1C81d0Ff96152b5AEcc3E78

VerifyJWT (orcid): 0x4D39C84712C9A13f4d348050E82A2Eeb45DB5e29

VerifyJWT (google): 0xC334b3465790bC77299D42635B25D77E3e46A78b

VerifyJWT (twitter): 0x97A2FAf052058b86a52A07F730EF8a16aD9aFcFB

VerifyJWT (github): 0x6029BD948942c7b355149087c47462c66Ea147ba

VerifyJWT (discord): 0xca6d00d3f78AD5a9B386824227BBe442c84344EA

# HTML Components
Holonym components you can use in your front end.

## Create Holonym Profile
Directs user to a page to create their Holo profile.

```html
<a href=”https://whoisthis.wtf/myholo”>Create your public profile</a>
```

