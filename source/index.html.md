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

[Download the JavaScript library from here](https://www.dropbox.com/s/jww6x2tfx9u1iuq/stardust.js?dl=0)

# Creating a Wallet

Create and store the wallet, it will be used for all actions.

```javascript
const { createWallet } = require('./stardust');
const [_address, _privateKey] = createWallet();

console.log('Wallet address: ', _address);
```

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

const getNonce = async (address) => Number((await axios.get(`/nonce/${address}`)).data.nonce);

const gameDataMaker = async (address) => ({
  'name' : 'Twilight Punkster Galaxy',
  'symbol' : 'TPG',
  'desc' : 'The Multiplayer Action RPG based on the Twilight Punkster comics.',
  'image' : 'branding-logo-twpunkster.png',
  'owner' : address,
  'nonce' : await getNonce(address)
});

const deployGame = async (game, privateKey) => {
  try {
    const query = await axios.post('/', createGamePostJSON(game, privateKey));
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

const testDeployGame = async () => {
  const _game = await gameDataMaker(_address);
  await deployGame(_game, _privateKey);
}

testDeployGame();
```

> The above command returns JSON structured like this:

```json
{
  "message": "Game was successfully deployed!",
  "gameData": {
    "desc": "The Multiplayer Action RPG based on the Twilight Punkster comics.",
    "image": "branding-logo-twpunkster.png",
    "name": "Twilight Punkster Galaxy",
    "nonce": 0,
    "owner": "0xBe8DFe1978549b8E4C55C7FFa088Bb1233771F71",
    "signedMessage": "0xd8f786c2377d53db8db1af0d3a0075e925057243788b2e28acb4f3bf1c266c4f341119d6567fb0a47cb734f4f717c07d087165fa12570660ae7a6d58bbbb489e1c7f47c1d3b84191d9c1dcc643af9836b9d17006a1962d5ae7bcb9682b2e9d8dd4",
    "symbol": "TPG"
  },
  "gameAddr": "0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
name | string | Game title | Twilight Punkster Galaxy
symbol | string | Game symbol | TPG
desc | string | Game description | The Multiplayer Action RPG based on the Twilight Punkster comics.
image | string | Game image | URL to image file
owner | string | Game owner (address) | 0xBe8DFe1978549b8E4C55C7FFa088Bb1233771F71 
nonce | number | The current number of transactions you have signed | 12

## Transfer Game Ownership

### HTTP Request

`POST http://api.stardust.cards/games/:gameAddr/transfer`

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const transferGameOwnership = async (gameAddr, transferData) => {
  try {
    const query = await axios.post(`/${gameAddr}/transfer`, transferData);
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95';

let transferData = {
  'owner': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce'
}

const testTransferGameOwnership = async () => {
  await transferGameOwnership(gameAddr, transferData);
}

testTransferGameOwnership();
```
> The above command returns JSON structured like this:

```json
{
  "owner": "0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameAddr | string | Game address | 0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95
owner | string | Game owner (address) | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce

## Get Balance Of Address

### HTTP Request

`GET http://api.stardust.cards/games/:gameAddr/balance/:userAddr`

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getBalanceOf = async (gameAddr, userAddr) => {
  try {
    const query = await axios.get(`/${gameAddr}/balance/${userAddr}`);
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95';
let userAddr = '0xC96AaF7Ac9eF529eB7B6118814b2951B53c4b2C4';

const testGetBalanceOf = async () => {
  await getBalanceOf(gameAddr, userAddr);
}

testGetBalanceOf();
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
userAddr | string | User address | 0xC96AaF7Ac9eF529eB7B6118814b2951B53c4b2C4

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
    const query = await axios.get('/');
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

const testGetGames = async () => {
  await getGames();
}

testGetGames();
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
    const query = await axios.get(`/${gameAddr}`);
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95';

const testGetGame = async () => {
  await getGame(gameAddr);
}

testGetGame();
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

const assetDataMaker = async(gameAddr, address) => ({
  'name': 'Furious Knuckles',
  'desc': 'Melee weapon with 6-8 damage. Low durability, yet lightweight and cheap.',
  'image': 'twitem-278.png',
  'rarity': 1,
  'cap': 0,
  'val': 66,
  gameAddr,
  'nonce': await getNonce(address),
});

const createAsset = async (asset, gameAddr, privateKey) => {
  try {
    const query = await axios.post(`/${gameAddr}/assets`, createAssetPostJSON(asset, privateKey));
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0xa509a89479B08F734Bd4bD16A762eDcE7Ba44D95';

const testCreateAsset = async () => {
  const _asset = await assetDataMaker(gameAddr, _address);
  await createAsset(_asset, gameAddr, _privateKey);
}

testCreateAsset();
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
name | string | Asset name | Furious Knuckles
desc | string | Asset description | Melee weapon with 6-8 damage. Low durability, yet lightweight and cheap.
image | string | Asset image | URL to image file
rarity | number | Asset rarity | 1
cap | number | How many can exist, 0 if unlimited | 0
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
nonce | number | The current number of transactions you have signed | 12

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
    const query = await axios.get(`/${gameAddr}/assets`);
    return query.data;
  } catch (e) {
    console.log(e)
  }
}

let gameAddr = '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce';

const testGetGameAssets = async () => {
  await getGameAssets(gameAddr);
}

testGetGameAssets();
```

> The above command returns JSON structured like this:

```json
{
  "assets": [
    {
      "assetId": 0,
      "cap": "0",
      "desc": "Melee weapon with 6-8 damage. Low durability, yet lightweight and cheap.",
      "gameAddr": "0x60dBAd46F93CF19CF8412f12454099bA088307f6",
      "image": "twitem-278.png",
      "name": "Furious Knuckles",
      "owners": [
        "0xCbacba158d237011Ca30088C5C49576D5317FdeB"
      ],
      "rarity": "1",
      "totalSupply": "1",
      "val": "66"
  },
  {
      "assetId": 1,
      "cap": "0",
      "desc": "Ranged weapon with 24-30 damage. Medium durability, heavy and a slow rate of recharge.",
      "gameAddr": "0x60dBAd46F93CF19CF8412f12454099bA088307f6",
      "image": "twitem-463.png",
      "name": "Gentech Super Buster X",
      "owners": [],
      "rarity": "3",
      "totalSupply": "0",
      "val": "123"
    }
  ],
  "message": "All created assets"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
assetId   | number  | Asset ID    | 1
cap       | number  | How many can exist, 0 if unlimited | 0
desc      | string  | Asset description | Melee weapon with 6-8 damage. Low durability, yet lightweight and cheap.
gameAddr  | string  | Asset address | 0x60dBAd46F93CF19CF8412f12454099bA088307f6
image     | string  | URL to image file  | 
name      | string  | Asset name  | Furious Knuckles
owners    | string array  | Asset owners | ["0xCbacba158d237011Ca30088C5C49576D5317FdeB"]
rarity    | number  | Asset rarity | 3 (0-5)
totalSupply | number  | Number of existing copies  | 1
val       | number  | Asset value | 66


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
    const query = await axios.get(`/${gameAddr}/assets/${assetId}`);
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0x60dBAd46F93CF19CF8412f12454099bA088307f6';
let assetId = 1;

const testGetSpecificAsset = async () => {
  await getSpecificAsset(gameAddr, assetId);
}

testGetSpecificAsset();
```

> The above command returns JSON structured like this:

```json
{
  "asset": {
    "assetId": "1",
    "cap": "0",
    "desc": "Ranged weapon with 24-30 damage. Medium durability, heavy and a slow rate of recharge.",
    "gameAddr": "0x60dBAd46F93CF19CF8412f12454099bA088307f6",
    "image": "twitem-463.png",
    "name": "Gentech Super Buster X",
    "owners": [],
    "rarity": "3",
    "totalSupply": "0",
    "val": "123"
  },
  "message": "Specific Asset details"
}
```

### Query Parameters

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
assetId | number | Asset ID | 5


## Retrieve Assets Of User

`GET http://api.stardust.cards/games/:gameAddr/assetsOf/:userAddr`

```javascript
const axios = require('axios');
axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getAssetsOf = async (gameAddr, userAddr) => {
  try {
    const query = await axios.get(`/${gameAddr}/assetsOf/${userAddr}`);
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0x60dBAd46F93CF19CF8412f12454099bA088307f6';

const testGetAssetsOf = async () => {
  await getAssetsOf(gameAddr, _address);
}

testGetAssetsOf();
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

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
userAddr | string | User address | 0x34E4be70A6763FddF14CBcF21f4e4902480638D2

## Minting Game Assets

`POST http://api.stardust.cards/games/:gameAddr/assets/:assetId/mint`

```javascript
const axios = require('axios');
const { createWallet, createAssetMintPostJSON } = require('./stardust');
const [_address, _privateKey] = createWallet();

axios.defaults.baseURL = 'http://104.248.225.156:3000/games';

const getNonce = async (address) => Number((await axios.get(`/nonce/${address}`)).data.nonce);

const assetMintDataMaker = async(gameAddr, assetId, userAddr) => ({
  gameAddr,
  assetId,
  'owner': _address,
  'nonce': await getNonce(_address),
  'to': userAddr,
  'amount': 10
});

const mintAsset = async (asset, gameAddr, assetId, privateKey) => {
  try {
    const query = await axios.post(`/${gameAddr}/assets/${assetId}/mint`, createAssetMintPostJSON(asset, privateKey));
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let gameAddr = '0x60dBAd46F93CF19CF8412f12454099bA088307f6';
let userAddr = '0x34E4be70A6763FddF14CBcF21f4e4902480638D2';
let assetId = 0;

const testMintAsset = async () => {
  const _asset = await assetMintDataMaker(gameAddr, assetId, userAddr);
  await mintAsset(_asset, gameAddr, assetId, _privateKey);
}

testMintAsset();
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

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
assetId | number | Asset ID | 5
to | string | Receiver address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
amount | number | Number of assets you are sending | 1
nonce | number | The current number of transactions you have signed | 12


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
    const query = await axios.post(`/${gameAddr}/assets/${assetId}/trade`, createAssetTradePostJSON(tradeData, privateKey));
    return query.data;
  } catch (e) {
    console.log(e);
  }
}

let assetId = 0;
let gameAddr = '0x60dBAd46F93CF19CF8412f12454099bA088307f6';
let to = '0xc75709080E584E6ba396FFe8ED7433f495339bA2';

const tradeDataMaker = async(assetId, to, address) => ({
  assetId,
  'to': to,
  'amount': 1,
  'nonce': await getNonce(address)
});

const testTradeAsset = async () => {
  const _tradeData = await tradeDataMaker(assetId, to, _address);
  await tradeAsset(_tradeData, gameAddr, _privateKey);
}

testTradeAsset();
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

Parameter | Type | Description | Example
--------- | ------- | ----------- | -------
gameAddr | string | Game address | 0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce
assetId | number | Asset ID | 5
to | string | Receiving address | 0xc75709080E584E6ba396FFe8ED7433f495339bA2
amount | number | Number of assets you are sending | 1
nonce | number | The current number of transactions you have signed | 12