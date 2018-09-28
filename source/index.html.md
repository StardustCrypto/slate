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

# Libraries

[Download the Javascript library from here](https://www.dropbox.com/s/jww6x2tfx9u1iuq/stardust.js?dl=0)

# Creating a Wallet

```javascript
const { createWallet } = require('./stardust');

let [_address, _privateKey] = createWallet();
```

# Get Balance Of Address

### HTTP Request

`GET http://api.stardust.cards/games/:gameAddr/balance/:userAddr`

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getBalanceOf = async (gameAddr, userAddr) => {
  try {
    let result = await axios.get(`/${gameAddr}/balance/${userAddr}`);
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95';
let userAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';

getBalanceOf(gameAddr, userAddr);
```
> The above command returns JSON structured like this:

```json
{
  "message": "Balance of address 0xC96AaF7Ac9eF529eB7B6118814b2951B53c4b2C4",
  "balance": "0"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameAddr | string | Game address | 0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95
userAddr | string | User address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce

# Game

## Create Your Game

### HTTP Request

`POST http://api.stardust.cards/games`

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
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const { createWallet, createGamePostJSON } = require('./stardust');
const [_address, _privateKey] = createWallet();

var _game = {
  'name' : 'Twilight Punkster Online',
  'symbol' : 'TPO',
  'desc' : 'Punk RPG',
  'image' : 'https://pbs.twimg.com/profile_images/1026578016258932737/RIV7fpWs_400x400.jpg',
  'owner' : _address,
  'nonce' : 0
};

const deployGame = async (game, privateKey) => {
  try {
    let result = await axios.post('/', createGamePostJSON(game, privateKey));
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

deployGame(_game, _privateKey);
```

> The above command returns JSON structured like this:

```json
{
  "message": "Game was successfully deployed!",
  "gameData": {
    "desc": "Punk RPG",
    "image": "https://pbs.twimg.com/profile_images/1026578016258932737/RIV7fpWs_400x400.jpg",
    "name": "Twilight Punkster Online",
    "nonce": 0,
    "owner": "0xBe8DFe1978549b8E4C55C7FFa088Bb1233771F71",
    "signedMessage": "0xd8f786c2377d53db8db1af0d3a0075e925057243788b2e28acb4f3bf1c266c4f341119d6567fb0a47cb734f4f717c07d087165fa12570660ae7a6d58bbbb489e1c7f47c1d3b84191d9c1dcc643af9836b9d17006a1962d5ae7bcb9682b2e9d8dd4",
    "symbol": "TPO"
  },
  "gameAddr": "0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
name | string | Game title | Twilight Punkster Online
symbol | string | Game symbol | TPO
desc | string | Game description | Punk RPG
image | string | Game image | C:\image.png (soon to be IPFS hash)
owner | string | Game owner (address) | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce 
nonce | integer | Index of your game | 0

## Transfer Game Ownership

### HTTP Request

`POST http://api.stardust.cards/games/:gameAddr/transfer`

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const transferGameOwnership = async (gameAddr, transferData) => {
  try {
    let result = await axios.post(`/${gameAddr}/transfer`, transferData);
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95';

let _transferData = {
  'owner': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce'
}

transferGameOwnership(gameAddr, _transferData);
```
> The above command returns JSON structured like this:

```json
{
  "owner": "0x1ec343a6eA64A3a651884a3E8ccBD45bF80A66FB"
}
```

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
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getGames = async () => {
  try {
    let result = await axios.get('/');
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

getGames();
```
> The above command returns JSON structured like this:

```json
{
  "games": [
    {
      "desc": "Punk RPG",
      "gameAddr": "0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95",
      "gameOwner": "0x0c69576D2FeC4572a74597386ab792Ba8a3706d2",
      "image": "https://pbs.twimg.com/profile_images/1026578016258932737/RIV7fpWs_400x400.jpg",
      "name": "Twilight Punkster Online",
      "rarityNames": [
        "Common",
        "Rare",
        "Super Rare",
        "Limited Edition",
        "Unique"
      ],
      "rarityPercs": [
        80,
        15,
        4,
        0.85,
        0.15
      ],
      "symbol": "TPO",
      "totalSupply": "1"
    }
  ],
  "message": "All currently deployed games"
}
```

## Retrieve Specific Game Data

### HTTP Request

`GET http://api.stardust.cards/games/:gameAddr`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key)

game_query_data = {'game_id': 5}

game_data = client.get_game(game_query_data)
```

```javascript
const axios = require('axios');

axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getGame = async (gameAddr) => {
  try {
    let result = await axios.get(`/${gameAddr}`);
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';
getGame(gameAddr);
```


> The above command returns JSON structured like this:

```json
{
  "game": {
    "desc": "Description",
    "gameAddr": "0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95",
    "gameOwner": "0x0c69576D2FeC4572a74597386ab792Ba8a3706d2",
    "image": "https://pbs.twimg.com/profile_images/1026578016258932737/RIV7fpWs_400x400.jpg",
    "name": "Twilight Punkster Online",
    "rarityNames": [
      "Common",
      "Rare",
      "Super Rare",
      "Limited Edition",
      "Unique"
    ],
    "rarityPercs": [
      80,
      15,
      4,
      0.85,
      0.15
    ],
    "symbol": "TPO",
    "totalSupply": "1"
  },
  "message": "Game details"
}
```

# Asset

## Creating Game Assets

### HTTP Request

`POST http://api.stardust.cards/games/:gameAddr/assets`

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
const axios = require('axios');
const { createWallet, createAssetPostJSON } = require('./stardust');;
const [_address, _privateKey] = createWallet();

axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getNonce = async (address) => Number((await axios.get(`/nonce/${address}`)).data.nonce);

const createAsset = async (asset, gameAddr, privateKey) => {
  try {
    let result = await axios.post(`/${gameAddr}/assets`, createAssetPostJSON(asset, privateKey));
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';

const _asset = {
  'name': 'AwoogaMonster',
  'desc': 'Killer AwoogaMonster',
  'image': 'AwoogaMonster.jpg',
  'rarity': 1,
  'cap': 0,
  'value': 500
  'gameAddr': gameAddr,
  'nonce': getNonce(_address),
};

createAsset(_asset, gameAddr, _privateKey);
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
name | string | Asset name | Awooga Cat
desc | string | Asset description | Cute and fluffy kitty
image | string | Asset image | IPFS image hash
rarity | string | Asset rarity | ultra rare
cap | integer | How many exist? -1 if unlimited | 10
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
nonce | integer | a | b

## Retrieve All Asset Data For a Game

### HTTP Request

`GET http://api.stardust.cards/games/:gameAddr/assets`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key) 

game_query_data = {'gameId': 5}

asset_data = client.get_assets(game_query_data)
```

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getGameAssets = async (gameAddr) => {
  try {
    let result = await axios.get(`/${gameAddr}/assets`);
    console.log(result.data);
  } catch (e) {
    console.log(e)
  }
}

let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';
getGameAssets(gameAddr);
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
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce


## Retrieve Specific Asset Data

### HTTP Request

`GET http://api.stardust.cards/games/:gameAddr/assets/:assetId`

```python
from stardust.wallet.client import Client

private_key = 'Enter your private key here'
client = Client(private_key) 

asset_query_data = {'gameId' : 5,
                    'assetId' : 1}

asset_data = client.get_asset_data(asset_query_data, asset_id)
```

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getSpecificAsset = async (gameAddr, assetId) => {
  try {
    let result = await axios.get(`/${gameAddr}/assets/${assetId}`);
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';
let assetId = 1;

getSpecificAsset(gameAddr, assetId);
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
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
assetId | integer | Asset ID | 5


## Retrieve Assets Of User

`GET http://api.stardust.cards/games/:gameAddr/assetsOf/:userAddr`

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getAssetsOf = async (gameAddr, userAddr) => {
  try {
    let result = await axios.get(`/${gameAddr}/assetsOf/${userAddr}`);
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';
let userAddr = '0x34E4be70A6763FddF14CBcF21f4e4902480638D2';

getAssetsOf(gameAddr, userAddr);
```
> The above command returns JSON structured like this:

```json
{
  "message": "Assets of user 0x34E4be70A6763FddF14CBcF21f4e4902480638D2",
  "assets": { "0": "1" },
  "info": "assetId:amountOfItem"
}
```

### Query Parameters

Parameter | Default | Description | Example
--------- | ------- | ----------- | -------
gameAddr | true | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
userAddr | true | User address | 0x34E4be70A6763FddF14CBcF21f4e4902480638D2

## Minting Game Assets

`POST http://api.stardust.cards/games/:gameAddr/assets/:assetId/mint`

```javascript
const axios = require('axios');
const { createWallet, createAssetMintPostJSON } = require('./stardust');
const [_address, _privateKey] = createWallet();

axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getNonce = async (address) => Number((await axios.get(`/nonce/${address}`)).data.nonce);

const mintAsset = async (asset, gameAddr, assetId, privateKey) => {
  try {
    let result = await axios.post(`/${gameAddr}/assets/${assetId}/mint`, createAssetMintPostJSON(asset, privateKey));
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0x0...';
let assetId = 1;

let _asset = {
  'gameAddr': gameAddr,
  'assetId': assetId,
  'owner': _address,
  'nonce': getNonce(_address),
  'to': '0x34E4be70A6763FddF14CBcF21f4e4902480638D2',
  'amount': 1
}

mintAsset(_asset, gameAddr, assetId, _privateKey);
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
gameAddr | true | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
assetId | true | Asset ID | 5
to | true | Receiver address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
amount | true | Number of assets you are sending | 1
nonce | true | a | 0


## Trading Game Assets

### HTTP Request

`POST http://api.stardust.cards/games/:gameAddr/assets/:assetId/trade`

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
const axios = require('axios');
const { createWallet, createAssetTradePostJSON } = require('./stardust');
const [_address, _privateKey] = createWallet();

axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getNonce = async (address) => Number((await axios.get(`/nonce/${address}`)).data.nonce);

const tradeAsset = async (tradeData, gameAddr, privateKey) => {
  try {
    let result = await axios.post(`/${gameAddr}/assets/${assetId}/trade`, createAssetTradePostJSON(tradeData, privateKey));
    console.log(result.data);
  } catch (e) {
    console.log(e);
  }
}

let assetId = 0;
let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';

let _tradeData = {
  assetId,
  'to': '0xc75709080E584E6ba396FFe8ED7433f495339bA2',
  'amount': 1,
  'nonce': getNonce(_address)
}

tradeAsset(_tradeData, gameAddr, _privateKey);
```

> The above command returns JSON structured like this:

```json
{
  "message": "Asset was successfully traded!",
  "from": "0x34E4be70A6763FddF14CBcF21f4e4902480638D2",
  "to": "0xc75709080E584E6ba396FFe8ED7433f495339bA2",
  "assetId": 0,
  "amount": 1
}
```

### Query Parameters

Parameter | Default | Description | Example
--------- | ------- | ----------- | -------
gameAddr | true | Game address | 1
assetId | true | Asset ID | 5
to | true | Address to send the asset to | 0xc75709080E584E6ba396FFe8ED7433f495339bA2
amount | true | Number of assets you are sending | 1
nonce | true | a | 0

