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

Welcome to the docs for the wtf-lib JavaScript library, a library that allows developers to query WTF protocol smart contracts.

# Configuration

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

# Accessor Functions


## getHolo(address)

> Get a user's holo (i.e., their name, bio, and all their credentials).

```javascript
const userHolo = await getHolo('0xdbd6b2c02338919edaa192f5b60f5e5840a50079')
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

### Parameters

Parameter | Description
--------- | -----------
address | A user's crypto address.


## getAllUserAddresses()

> Get every crypto address that has registered on a WTF contract.

```javascript
const userAddresses = await getAllUserAddresses()
```

> Returns

```json
{
    "gnosis": {
        "orcid": [
            "0xc8834c1fcf0df6623fc8c8ed25064a4148d99388",
            "0xdbd6b2c02338919edaa192f5b60f5e5840a50079", 
        ],
        "google": [
            "0xc8834c1fcf0df6623fc8c8ed25064a4148d99388",
            "0xdbd6b2c02338919edaa192f5b60f5e5840a50079", 
        ],
        "twitter": [
            "0xc8834c1fcf0df6623fc8c8ed25064a4148d99388",
            "0xdbd6b2c02338919edaa192f5b60f5e5840a50079", 
        ],
        "github": [
            "0xc8834c1fcf0df6623fc8c8ed25064a4148d99388",
            "0xdbd6b2c02338919edaa192f5b60f5e5840a50079", 
        ],
        "discord": [],
        "nameAndBio": [
            "0xc8834c1fcf0df6623fc8c8ed25064a4148d99388",
            "0xdbd6b2c02338919edaa192f5b60f5e5840a50079", 
        ],
    }
}
```

This function returns every crypto address that has registered on a WTF contract. 

It sorts users by network and service. It does not remove duplicates, so if a user has linked their crypto address with their ORCID ID and their gmail, their address will show up in both the "orcid" list and the "google" list.


## credentialsForAddress(address, service)

> Get a user's gmail.

```javascript
const userGmail = await credentialsForAddress('0xdbd6b2c02338919edaa192f5b60f5e5840a50079', 'google')
```

> Returns

```javascript
'thejerrygerbil@gmail.com'
```

This function returns the user's credentials (e.g., their ORCID ID or gmail), given the user's crypto address. 

### Parameters

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
const userAddress = await addressForCredentials('thejerrygerbil@gmail.com', 'google')
```

> Returns

```javascript
'0xdbd6b2c02338919edaa192f5b60f5e5840a50079'
```

This function returns the user's crypto address, given their credentials (e.g., their ORCID ID or gmail). 

### Parameters

Parameter | Description
--------- | -----------
credentials | A user's credentials.
service | The service that issued the credentials (e.g., ORCID or Google).


## nameForAddress(address)

> Get a user's name.

```javascript
const userName = await nameForAddress('0xdbd6b2c02338919edaa192f5b60f5e5840a50079')
```

> Returns

```javascript
'Jerry'
```

This function returns the user's name, given their address.

### Parameters

Parameter | Description
--------- | -----------
address | A user's crypto address.

## bioForAddress(address)


> Get a user's bio/description.

```javascript
const userBio = await bioForAddress('0xdbd6b2c02338919edaa192f5b60f5e5840a50079')
```

> Returns

```javascript
'The gerbil (the real one) 🐹'
```

This function returns the user's bio/description, given their address.

### Parameters

Parameter | Description
--------- | -----------
address | A user's crypto address.

