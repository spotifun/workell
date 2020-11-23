# workell

Cloudflare Worker with Haskell and WebAssembly for Spotifun API.

## Build

```shell
docker run -it --rm -v $(pwd):/workspace -w /workspace terrorjack/asterius:200617
ahc-link \
  --input-hs fact.hs \
  --no-main \
  --export-function=fact \
  --input-mjs fact_cfw.mjs \
  --bundle \
  --browser \
  --output-dir worker
```

## Deploy

```shell
cd worker
curl -X PUT "https://api.cloudflare.com/client/v4/accounts/$CF_ACCOUNT_ID/workers/scripts/$SCRIPT_NAME" \
     -H  "Authorization: Bearer $CF_API_TOKEN" \
     -F "metadata=@metadata.json;type=application/json" \
     -F "script=@fact.js;type=application/javascript" \
     -F "wasm=@fact.wasm;type=application/wasm"
```

## Testing

```shell
curl -X POST $CFW_SUBDOMAIN \
       -H 'Content-Type: application/x-www-form-urlencoded' \
       --data 'param=5'
```
