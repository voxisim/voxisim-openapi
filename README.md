# Voxisim OpenAPI

The OpenAPI 3.0 description of the [Voxisim](https://voxisim.com) REST API. Use it to buy one-time SMS verification numbers and to rent numbers for longer-term SMS. You can pay from an account balance, or per request in USDC over [x402](https://www.x402.org).

|                |                                                                                            |
| -------------- | ------------------------------------------------------------------------------------------ |
| Spec           | [`openapi.json`](openapi.json), also served live at <https://api.voxisim.com/openapi.json> |
| Base URL       | `https://api.voxisim.com`                                                                  |
| API reference  | <https://voxisim.com/docs/api> ([Markdown](https://voxisim.com/docs/api.md))               |
| Product guides | <https://voxisim.com/docs>                                                                 |
| MCP server     | <https://voxisim.com/docs/mcp.md>                                                          |

## Authentication

Authenticated requests carry a bearer token. It can be a `vxs_` API key or an OAuth access token:

```
Authorization: Bearer vxs_...
```

You can get a key in two ways:

- **Sign up.** Call `POST /v1/signup`. The API key comes back once, so store it straight away.
- **Pay per request.** Skip the account. Send a purchase with no credentials, sign one of the payment options in the `402` response, then repeat the request with an `X-PAYMENT` header. The first payment creates an account tied to your wallet and returns its API key once, as `receipt_token`.

The discovery endpoints (services, countries, prices) work without authentication.

## Quick start

```sh
# Services you can get a one-time SMS code for
curl https://api.voxisim.com/v1/sms/activation/services

# Countries and live prices for one service
curl "https://api.voxisim.com/v1/sms/activation/countries?service_id=<service_id>"

# Your balance
curl https://api.voxisim.com/v1/balance \
  -H "Authorization: Bearer $VOXISIM_API_KEY"
```

Prices and availability change all the time. Read them from the discovery endpoints and never hardcode them.

## Generating a client

The spec works with standard OpenAPI tooling, for example:

```sh
npx @openapitools/openapi-generator-cli generate \
  -i https://raw.githubusercontent.com/voxisim/voxisim-openapi/main/openapi.json \
  -g typescript-fetch -o ./voxisim-client
```

You can also import `openapi.json` straight into Postman, Insomnia or Bruno.
