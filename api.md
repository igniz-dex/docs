> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/api.md).

# API

> **Note:** This is a sample page. The full API reference will be imported here later.

#### Introduction <a href="#introduction" id="introduction"></a>

The Igniz API lets developers integrate market data, account information, and trading functionality into their own applications. This page will host the complete reference, including authentication, endpoints, rate limits, and code examples.

#### Sample: Base URL <a href="#sample-base-url" id="sample-base-url"></a>

```
https://api.igniz.example/v1
```

#### Sample: Authenticated Request <a href="#sample-authenticated-request" id="sample-authenticated-request"></a>

```http
GET /v1/markets/ticker?symbol=BTCUSDT
Authorization: Bearer <API_KEY>
```

```json
{
  "symbol": "BTCUSDT",
  "lastPrice": "0.00",
  "priceChangePercent": "0.00"
}
```

> The endpoints, parameters, and payloads shown above are illustrative placeholders only. Final, authoritative API documentation will replace this page.
