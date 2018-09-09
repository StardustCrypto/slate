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

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

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

`POST http://example.com/games/deploy`

```python
from stardust.wallet.client import Client

client = Client(private_key)

my_game_data = {
    'name': 'CryptoKitties'
    'desc': 'Trading cats on the blockchain',
    'image': '',
    'nonce': 0,
    'owner': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce',
    'symbol': 'CAT'
};

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
image | string | Image of your game | C:\image.png
owner | string | Owner address | 0x0
nonce | integer | Index of your game | 0
signedMessage | string | Use the hashAndSignGame function | 

## Retrieve All Game Data

### HTTP Request

`GET http://example.com/games`

```ruby
require 'stardust/wallet'

client = Stardust::Wallet::Client.new(priv_key: private_key)
game_data = client.games
```

```python
from stardust.wallet.client import Client

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

`GET http://example.com/games/:gameId`

```python
from stardust.wallet.client import Client

client = Client(private_key, game_id)
game_data = client.get_game(game_id)
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

# Assets

## Creating Game Assets

### HTTP Request

`POST http://example.com/games/:gameId/create`

```python
from stardust.wallet.client import Client

client = Client(private_key, game_id)

my_asset_data = {
    'name': 'Awooga Cat'
    'desc': 'Cute and fluffy kitty',
    'image': '',
    'rarity': 'ultra rate',
    'cap': 10
    'gameAddr': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce',
    'nonce': 0
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

Parameter | Default | Description | Example
--------- | ------- | ----------- | -------
name | false | Name of your asset | Awooga Cat
desc | true | Description of your asset | Cute and fluffy kitty
image | true | Image of your game | example image
rarity | true | How rare is the asset | ultra rare
cap | true | How many exist? -1 if unlimited | 10
gameAddr | true | Address of game | 0x232323
nonce | true | a | b

## Retrieve All Asset Data

### HTTP Request

`POST http://example.com/games/:gameId/assets`

```python
from stardust.wallet.client import Client

client = Client(private_key, game_id) 

asset_data = client.get_assets()
```

> The above command returns JSON structured like this:

```json
{
   "message": "Asset Created"
}
```

## Retrieve Specific Asset Data

### HTTP Request

`POST http://example.com/games/:gameId/assets/:cardId`

```python
from stardust.wallet.client import Client

client = Client(private_key, game_id) 

asset_id = {'asset_id' : 1}

asset_data = client.get_asset_data(asset_id)
```

> The above command returns JSON structured like this:

```json
{
   "message": "Asset Created"
}
```


## Trading Game Assets

### HTTP Request

`POST http://example.com/games/:gameId/trade`

```python
from stardust.wallet.client import Client

client = Client(private_key, game_id)

trade_data = {
    'gameId': 1,
    'assetId': 5,
    'to','0x0',
    'amount': 1,
    'nonce': 0,
};

signed_trade_data = client.hashAndSignTade(trade_data)

game_data = client.create_asset(trade_data, signed_trade_data)
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



# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

