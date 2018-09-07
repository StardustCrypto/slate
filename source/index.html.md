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

# Creating your Game

> To create your game, use this code:

```ruby
none yet
```

```python
none yet
```

```shell
none yet
```

```javascript

'use strict';
const request = require('request');
const Web3 = require('web3');
const web3 = new Web3();

const{'utils': {soliditySha3}, 'eth': {abi, accounts}} = web3;

const hashParam = (value, type) => soliditySha3(abi.encodeParameter(type, value));
const hashParams = (values, types) => values.map((value, index) => hashParam(value, types[index]));

const hashGame = (gameData) => {
    const{name, symbol, desc, image, owner, nonce} = gameData;
    const hashes = hashParams([name, symbol, desc, image, owner, nonce], ['string', 'string', 'string', 'string', 'address', 'uint256']);
    return(hashParam(hashes, 'bytes32[]'));
};

const hashCard = ({name, desc, image, rarity, cap, gameAddr, nonce}) => {
    const hashes = hashParams([name, desc, image, cap, rarity, gameAddr, nonce], ['string', 'string', 'string', 'uint256', 'uint256', 'address', 'uint256']);
    return(hashParam(hashes, 'bytes32[]'));
};

const hashAndSignGame = (gameData, privKey) => {
    const sig = accounts.sign(hashGame(gameData), privKey);
    return({'signedMessage': sig.signature + sig.message.slice(2)});
};

const hashAndSignCard = (cardData, privKey) => {
    const sig = accounts.sign(hashCard(cardData), privKey);
    return({'signedMessage': sig.signature + sig.message.slice(2)});
};

const createGamePostJSON = (gameData, privKey) => ({...gameData, ...hashAndSignGame(gameData, privKey)});
const createCardPostJSON = (cardData, privKey) => ({...cardData, ...hashAndSignCard(cardData, privKey)});

const _game = {
    'desc': '',
    'image': '',
    'name': '',
    'nonce': 0,
    'owner': '0xbA418a52A50c7169dbf7296D64B45a82DFa093Ce',
    'symbol': ''
};

const _card = {
    'cap': 0,
    'desc': '',
    'gameAddr': '',
    'image': '',
    'name': '',
    'nonce': 1,
    'rarity': 1
};

const _privateKey = '0x7023ae66f4de385e8f9f01ee94456a7fd7fd3db70fdc51cea26833fa4a08ca55';


const ip = '';
const port = 3000;
const baseURL = 'games';

const sendGame = (game, privateKey) => {
    request.post(
        `${ip}:${port}/${baseURL}/deploy`,
        {'json': createGamePostJSON(game, privateKey)},
        (error, response, body) => { if(!error && response.statusCode == 200) { console.log(error, response, body); } }
    );
};

const sendCard = (card, gameId, privateKey) => {
    request.post(
        `${ip}:${port}/${baseURL}/${gameId}/mint`,
        {'json': createCardPostJSON(card, privateKey)},
        (error, response, body) => { if(!error && response.statusCode == 200) { console.log(error, response, body); } }
    );
};

```

this is a test description for the code on the right

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

## Awooga Kittens

chicken nuggetss

<aside class="notice">
You must replace <code>asd</code> awooooga
</aside>

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

