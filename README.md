# Redis Shopping Cart demo

This shopping cart is using Redis and RedisJson module functionalities, allowing you to save JSON as keys using methods like json_get and json_set

![alt text](https://github.com/redis-developer/basic-redis-shopping-chart-nodejs/blob/main/preview.png?raw=true)

## How it works

### How the data is stored:
* The products data is stored in external json file. After first request this data is saved in a JSON data type in Redis like: `JSON.SET product:{productId} . {product data in json format}`.
* The cart data is stored in a hash like: `HSET cart:{cartId} product:{productId} {productQuantity}`, where cartId is random generated value and stored in user session.

### How the data is modified:
* The product data is modified like `JSON.SET product:{productId} . {new product data in json format}`.
* The cart data is modified like `HSET cart:{cartId} product:{productId} {newProductQuantity}` or `HINCRBY cart:{cartId} product:{productId} {incrementBy}`.
* Product can be removed from cart like `HDEL cart:{cartId} product:{productId}`
* Cart can be cleared using `HGETALL cart:{cartId}` and then `HDEL cart:{cartId} {productKey}`.
* All carts can be deleted when reset data is requested like: `DEL cart:{cartId}`.

### How the data is accessed:
* Products: `SCAN {cursor} MATCH product:*` to get all product keys and then `JSON.GET {productKey}`.
* Cart: `HGETALL cart:{cartId}`to get quantity of products and `JSON.GET product:{productId}` to get products data.

## Hot to run it locally?

### Prerequisites

- Node - v12.19.0

```
% brew install node
```

```
node 15.5.0 is already installed
```

- NPM - v6.14.8

```
 % npm version
{
  npm: '7.3.0',
```

- Docker - v19.03.13 (optional)

### Local installation

Go to server folder and then:

```
# Environmental variables

Copy `.env.example` to `.env` file and fill environmental variables

REDIS_PORT: Redis port (default: 6379)
REDIS_HOST: Redis host (default: 127.0.0.1)
REDIS_PASSWORD: Redis password (default: demo)

# Run docker compose or install redis with RedisJson module manually. You can also go to https://redislabs.com/try-free/ and obtain necessary environmental variables

docker network create global
docker-compose up -d --build
```

The output should display:

```
docker-compose ps
             Name                           Command               State             Ports          
---------------------------------------------------------------------------------------------------
redis.redisshoppingcart.docker   docker-entrypoint.sh redis ...   Up      127.0.0.1:55000->6379/tcp
```

```
% docker-compose logs -f
Attaching to redis.redisshoppingcart.docker
redis.redisshoppingcart.docker | 1:C 14 Feb 2021 15:39:02.178 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis.redisshoppingcart.docker | 1:C 14 Feb 2021 15:39:02.178 # Redis version=6.0.9, bits=64, commit=00000000, modified=0, pid=1, just started
redis.redisshoppingcart.docker | 1:C 14 Feb 2021 15:39:02.178 # Configuration loaded
redis.redisshoppingcart.docker | 1:M 14 Feb 2021 15:39:02.179 * Running mode=standalone, port=6379.
redis.redisshoppingcart.docker | 1:M 14 Feb 2021 15:39:02.179 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis.redisshoppingcart.docker | 1:M 14 Feb 2021 15:39:02.179 # Server initialized
redis.redisshoppingcart.docker | 1:M 14 Feb 2021 15:39:02.179 # <ReJSON> JSON data type for Redis v1.0.7 [encver 0]
redis.redisshoppingcart.docker | 1:M 14 Feb 2021 15:39:02.179 * Module 'ReJSON' loaded from /usr/lib/redis/modules/rejson.so
redis.redisshoppingcart.docker | 1:M 14 Feb 2021 15:39:02.180 * Ready to accept connections
```



# Install dependencies

```
npm cache clean && npm install
```

# Run dev server

```
npm run dev
```

Go to client folder and then:

```
# Environmental variables

Copy `.env.example` to `.env` file

# Install dependencies

npm cache clean && npm install

# Serve locally

npm run serve
```

## Deployment

To make deploys work, you need to create free account in https://redislabs.com/try-free/, create Redis instance with `RedisJson` module and get informations - REDIS_ENDPOINT_URI and REDIS_PASSWORD. You must pass them as environmental variables.

### Google Cloud Run

[![Run on Google
Cloud](https://deploy.cloud.run/button.svg)](https://deploy.cloud.run/?git_repo=https://github.com/redis-developer/basic-redis-shopping-chart-nodejs.git)

### Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

### Vercel

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/git/external?repository-url=https://github.com/redis-developer/basic-redis-shopping-chart-nodejs&env=REDIS_ENDPOINT_URI,REDIS_PASSWORD)
