# Opium Exchange socket.io endpoint specification

## Version

v1.0.0

Use `socket.io` namespace `v1`

## Endpoint

[https://api.opium.exchange/v1](https://api.opium.exchange/v1)

## Description

Opium Exchange socket.io endpoint allows clients to receive immediate updates on products' charts and users' orders, swaps and positions on third party projects

# Channels
## Data channel `chart:asset`

Asset chart data

### Parameters
```
{
  o: String - oracleId,
  s: String - candle size 1$m/30$m/1$h/2$h/4$d/12$h/1$d/3$d/5$d/1$w/all
}
```

### Response message
```
{
  ch: 'chart:asset',
  a: 'set',
  p: { o, s },
  d: [{
    o: Number - open
    h: Number - high
    l: Number - low
    c: Number - close
    v: Number - volume
    t: Number - timestamp
  }]
}
```

## Data channel `chart:deriv`

Derivative chart data

### Parameters
```
{
  t: String - ticker hash,
  s: String - candle size 1$m/30$m/1$h/2$h/4$d/12$h/1$d/3$d/5$d/1$w/all
  c: String - currency
}
```

### Response message
```
{
  ch: 'chart:deriv',
  a: 'set',
  p: { t, s, c },
  d: [{
    o: Number - open
    h: Number - high
    l: Number - low
    c: Number - close
    v: Number - volume
    t: Number - timestamp
  }]
}
```

## Data channel `orderbook:orders:ticker`

Whole orderbook state on every update

### Parameters
```
{
  t: String - ticker hash,
  c: String - currency
}
```

### Response message
```
{
 ch: 'orderbook:orders:ticker',
 a: 'set',
 p: { t, c }
 d: [{
   a: String - action BID, ASK
   p: Number - price,
   v: Number - volume,
   c: Number - commulative
 }]
}
```
## Data channel `orderbook:orders:makerAddress`

List of orders for makerAddress on every update
Signature (access token) check is required

### Parameters
```
{
  t: String - ticker hash
  c: String - currency
  addr: String - maker address to fetch orders for,
  sig: String - Signature (access token)
}
```

### Response message
```
{
 ch: 'orderbook:orders:makerAddress',
 a: 'set',
 p: { t, c, addr },
 d: [{
   i: String - Order id
   a: String - action BID, ASK
   p: Number - price,
   q: Number - quantity,
   f: Number - filled,
   m: Boolean - matching
   cT: Number - createdAt,
   eT: Number - expiresAt
 }]
}
```
## Data channel `orderbook:orders:overview`

List of derivative trading overview on every update

### Parameters
```
{
  t: String - type: option, cds,
  o: String - oracleId
}
```

### Response message
```
{
 ch: 'orderbook:orders:overview',
 a: 'set',
 d: [{
   t: String - ticker hash,
   aS: Number - ask size,
   aP: Number - ask price,
   bS: Number - bid size,
   bP: Number - bid price
 }]
}
```
## Data channel `positions:address`

Positions of given address on every update
Signature (access token) check is required

### Parameters
```
{
  addr: String - public key to fetch positions for,
  sig: String - Signature (access token)
}
```

### Response message
```
{
 ch: 'positions:address',
 a: 'set',
 p: { addr },
 d: { // data
  pos: [{ // positions
    q, // quantity
    ti, // tokenId
    ty: /LONG,SHORT/, // type
    tt // title
    ex: Boolean // Executable
    hd: Boolean // has data,
    th: String // Ticker hash,
    ca: Boolean // Cancel
  }],
  por: [{ // portfolios
    q, // quantity
    ti, // tokenId
    pos: [{ // positions
      r, // ratio
      ti, // tokenId
      ty: /LONG,SHORT/, // type
      tt, // title
      th: String // Ticker hash
      ca: Boolean // Cancel
    }]
  }]
 }
}
```
## Data channel `trades:ticker:all`

Trade list on every update

### Parameters
```
{
  t: String - ticker hash,
  c: String - currency
}
```

### Response message
```
{
 ch: 'trades:ticker:all',
 p: { t, c }, - params
 a: 'set',
 d: [{
   t: String - ISODate
   a: String - action BID, ASK
   p: Number - price,
   q: Number - quantity,
   tx: String - txHash
 }]
}
```
## Data channel `trades:ticker:address`

Trades of address on every update
Signature (access token) check is required

### Parameters
```
{
  t: String - ticker hash
  c: String - currency
  addr: String - maker address to fetch orders for,
  sig: String - Signature (access token)
}
```

### Response message
```
{
 ch: 'trades:ticker:address',
 p: { t, c, addr }, - params
 a: 'set',
 d: [{
   t: String - ISODate
   a: String - action BID, ASK
   p: Number - price,
   q: Number - quantity,
   tx: String - txHash
 }]
}
```
## Data channel `derivative:stats`

Derivative stats on every update

### Parameters
```
{
  t: String - ticker hash,
  c: String - currency
}
```

### Response message
```
{
 ch: 'derivative:stats',
 a: 'set',
 p: { t, c },
 d: [{
   v: Number - 24h volume,
   h: Number - 24h high,
   l: Number - 24h low
 }]
}
```
