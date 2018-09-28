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

Download the Javascript Library from 

https://www.dropbox.com/s/jww6x2tfx9u1iuq/stardust.js

# Libraries

Please download the associated module for your language on the right! A

# Creating a Wallet

```javascript
let [_publicKey, _privateKey] = stardust.createWallet()
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

```javascript
const stardust = require('./stardust')

let [_publicKey, _privateKey] = stardust.createWallet()

var _game = {
  'name' : 'Twilight Punkster Online',
  'symbol' : 'RPG',
  'desc' : 'Punk RPG',
  'image' : 'https://pbs.twimg.com/profile_images/1026578016258932737/RIV7fpWs_400x400.jpg',
  'owner' : _publicKey,
  'nonce' : 0
};

const sendGame = (game, privateKey) => {
    request.post(
        `http://${ip}:${port}/${baseURL}/deploy`,
        {'json': stardust.createGamePostJSON(game, privateKey)},
        (error, response, body) => { if(!error && response.statusCode != 200) { console.log(error, response, body); } else { console.log(response.statusCode, body);
    );
};

sendGame(_game, _publicKey)
```


> The above command returns JSON structured like this:

```json
{
  "message": "Game was successfully deployed!",
  "gameData": {
    "desc": "a test game",
    "image": "https://pbs.twimg.com/profile_images/1026578016258932737/RIV7fpWs_400x400.jpg",
    "name": "Twilight Punkster Online",
    "nonce": 0,
    "owner": "0x103eD377D3b0D1602D825D55DD677B2Aa93385eE",
    "signedMessage": "0xde05cda5447e7f697b3bbffcafeacb369c6f5618e728cca6d714a8537205c1f70a552af4751dac62f801f1458735b582225d9a5d1f0a5e3cd218c3d3315b22821c163c6c5deeac27365a52f25329d91f0b561c861d101aa3d34a2972c3ca4e8a88",
    "symbol": ""
  },
  "gameAddr": "0xE10A26f31aa3D8e5ccb409eFF01D62f36Da309A1"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
name | string | Name of your game | CryptoKitties
symbol | string | Short symbol of your game | Kitty
desc | string | Description of your game | Buy and sell crypto cats
image | string | Image of your game | C:\image.png (soon to be IPFS hash)
owner | string | Owner address (public key) | 0x0 
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

```javascript
const stardust = require('./stardust')

const getGames = () => {
    request.get(
        `http://${ip}:${port}/${baseURL}/`,
        (error, response, body) => { if(!error && response.statusCode != 200) { console.log(error, response, body); } else { console.log(response.statusCode, body);} }
    );
};

getGames();
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
      }
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

```javascript
const stardust = require('./stardust')

const getGame = (gameId) => {
    request.get(
        `http://${ip}:${port}/${baseURL}/${gameId}`,
        (error, response, body) => { if(!error && response.statusCode != 200) { console.log(error, response, body); } else { console.log(response.statusCode, body);} }
    );
};

var _gameId = 5;
getGame(_gameId);
```


> The above command returns JSON structured like this:

```json
{
   "game":{
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
   "message":"Game details"
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

```javascript
const stardust = require('./stardust')

const _asset = {
        'cap': 0,
        'desc': 'Killer AwoogaMonster',
        'gameAddr': gameAddr,
        'image': 'AwoogaMonster.jpg',
        'name': 'AwoogaMonster',
        'nonce': 1,
        'rarity': 1,
        'value': 500
    };

const sendAsset = (asset, gameAddr, privateKey) => {
    request.post(
        `http://${ip}:${port}/${baseURL}/${gameAddr}/create`,
        {'json': stardust.createAssetPostJSON(asset, privateKey)},
        (error, response, body) => { if(!error && response.statusCode != 200) { console.log(error, body); } else { console.log(response.statusCode, body);} }
    );
};

var gameAddr = '0x03423'

```

> The above command returns JSON structured like this:

```json
{
  "assetId": 1,
  "message": "Asset was successfully added!"
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


## Minting Game Assets

`POST http://api.stardust.cards/games/:gameId/trade`

```javascript
const sendAssetMint = (asset, gameAddr, assetId, privateKey) => {
    request.post(
        `http://${ip}:${port}/${baseURL}/${gameAddr}/assets/${assetId}/mint`,
        {'json': stardust.createAssetMintPostJSON(asset, privateKey)},
        (error, response, body) => { if(!error && response.statusCode != 200) { console.log(error, body); } else { console.log(response.statusCode, body);} }
    );
};
```
> The above command returns JSON structured like this:

```json
{
  "message": "Asset was successfully minted!",
  "mintData": {
    "assetId": 1,
    "to": "0x34E4be70A6763FddF14CBcF21f4e4902480638D2",
    "amount": 1
  }
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

```javascript
const tradeAsset = (tradeData, gameAddr, privateKey) => {
    request.post(
        `http://${ip}:${port}/${baseURL}/${gameAddr}/trade`,
        {'json': stardust.createAssetTradePostJSON(tradeData, privateKey)},
        (error, response, body) => { if(!error && response.statusCode != 200) { console.log(error, body); } else { console.log(response.statusCode, body);} }
    );
};
```

> The above command returns JSON structured like this:

```json
{
  "message": "Asset was successfully traded!",
  "from": "0x34E4be70A6763FddF14CBcF21f4e4902480638D2",
  "to": "0xc75709080E584E6ba396FFe8ED7433f495339bA2",
  "assetId": 1,
  "amount": 1
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

