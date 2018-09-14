---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Stardust Platform! You can use our libraries to easily create ERC-721 assets and utilize them within your game.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Hashing

```javascript
'use strict';
const request = require('request');
const Web3 = require('web3');
const web3 = new Web3();

const hashCard = (data, types) => {
    const hashes = hashParams(data, types);
    return(hashParam(hashes, 'bytes32[]'));
};
```

# Creating your Game

## Create Your Game

### HTTP Request

`POST http://api.stardust.cards/games/deploy`

```python
from stardust.wallet.client import Client


private_key = 'Enter your private key here'
client = Client(private_key)

my_game_data = {
    'name': 'CryptoKitties'
    'desc': 'Trading cats on the blockchain',
    'image': '', #soon to be IPFs hash
    'nonce': 0,
    'owner': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce',
    'symbol': 'CAT'
}

signed_game_hash = client.hashAndSignGame(my_game_data)
create_game_response = client.create_game(my_game_data, signed_game_hash)
```

> The above command returns JSON structured like this:

```json
{
   "message": "Game Created"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
name | string | Name of your game | CryptoKitties
symbol | string | Shorty symbol of your game | Kitty
desc | string | Description of your game | Buy and sell crypto cats
image | string | Image of your game | C:\image.png (soon to be IPFS hash)
owner | string | Owner address | 0x0
nonce | integer | Index of your game | 0

## Retrieve All Game Data

### HTTP Request

`GET http://api.stardust.cards/games`

```ruby
require 'stardust/wallet'

client = Stardust::Wallet::Client.new(priv_key: private_key)
game_data = client.games
```

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key)

game_data = client.get_games()
```

> The above command returns JSON structured like this:

```json
{
   "games":[
      {
         "desc":"game_desc",
         "gameContractAddress":"0x...",
         "gameId":0,
         "gameOwner":"0x...",
         "image":"image_link", 
         "name":"game_name",
         "rarityNames":[
            "Common",
            "Rare",
            "Super Rare",
            "Limited Edition",
            "Unique"
         ],
         "rarityPercs":[
            80,
            15,
            4,
            0.85,
            0.15
         ],
         "symbol":"game_symbol",
         "totalSupply":"0"
      },
   ],
   "message":"All currently deployed games"
}
```

## Retrieve Specific Game Data

### HTTP Request

`GET http://api.stardust.cards/games/:gameId`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key)

game_query_data = {'game_id': 5}

game_data = client.get_game(game_query_data)
```

> The above command returns JSON structured like this:

```json
{
   "desc":"game_desc",
   "gameContractAddress":"0x...",
   "gameId":0,
   "gameOwner":"0x...",
   "image":"image_link",
   "name":"game_name",
   "rarityNames":[
      "Common",
      "Rare",
      "Super Rare",
      "Limited Edition",
      "Unique"
   ],
   "rarityPercs":[
      80,
      15,
      4,
      0.85,
      0.15
   ],
   "symbol":"game_symbol",
   "totalSupply":"0"
}
```

# Assets

## Creating Game Assets

### HTTP Request

`POST http://api.stardust.cards/games/:gameId/create`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key)

my_asset_data = {
    'name': 'Awooga Cat'
    'desc': 'Cute and fluffy kitty',
    'image': '', #soon to be IPFs hash
    'rarity': 'ultra rare',
    'cap': 10
    'gameAddr': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce',
    'nonce': 0,
    'gameId': 5
};

signed_asset_hash = client.hashAndSignAsset(my_asset_data)
game_data = client.create_asset(my_asset_data, signed_asset_hash)
```

> The above command returns JSON structured like this:

```json
{
   "message": "Asset Created"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
name | string | Name of your asset | Awooga Cat
desc | string | Description of your asset | Cute and fluffy kitty
image | string | Image of your game | IPFS image hash
rarity | string | How rare is the asset | ultra rare
cap | integer | How many exist? -1 if unlimited | 10
gameAddr | string | Address of game | 0x232323
nonce | integer | a | b
gameId | integer | Game ID that this asset belongs to | 5

## Retrieve All Asset Data For a Game

### HTTP Request

`POST http://api.stardust.cards/games/:gameId/assets`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key) 

game_query_data = {'gameId': 5}

asset_data = client.get_assets(game_query_data)
```

> The above command returns JSON structured like this:

```json

[
  {
    "id": 1,
    "name": "Snuggles",
    "breed": "Satoshi",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Barry Allen",
    "breed": "Superhero",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameId | integer | ID of the game | 1


## Retrieve Specific Asset Data

### HTTP Request

`POST http://api.stardust.cards/games/:gameId/assets/:cardId`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key) 

asset_query_data = {'gameId' : 5,
                    'assetId' : 1}

asset_data = client.get_asset_data(asset_query_data, asset_id)
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Snuggles",
    "breed": "Satoshi",
    "fluffiness": 6,
    "cuteness": 7
  }
]
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameId | integer | ID of the game | 1
assetId | integer | ID of the asset | 5


## Trading Game Assets

### HTTP Request

`POST http://api.stardust.cards/games/:gameId/trade`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key)

trade_data = {'gameId': 1,
              'assetId' : 5,
              'to': '0x123',
              'amount': 1,
              'nonce': 0}

signed_asset_hash = client.hashAndSignTrade(trade_data)
game_data = client.trade_assets(trade_data)
```

> The above command returns JSON structured like this:

```json
{
   "message": "Trade successful"
}
```

### Query Parameters

Parameter | Default | Description | Example
--------- | ------- | ----------- | -------
gameId | false | ID of the game | 1
assetId | true | ID of the asset | 5
to | true | Address to send the asset to | 0x0
amount | true | Number of assets you are sending | 1
nonce | true | a | 0

