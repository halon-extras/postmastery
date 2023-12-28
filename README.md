# postmastery_smtp($arguments, $message, $redact = false)
This function converts a SMTP delivery result into a format that is compatible with Postmastery's Delivery Analytics product.

## Params

- `$arguments` The [pre-defined](https://docs.halon.io/hsl/postdelivery.html#v-z1) `$arguments` variable **Required**
- `$message` The [pre-defined](https://docs.halon.io/hsl/postdelivery.html#v-m1) `$message` variable **Required**
- `$redact` If the recipient local part should be redacted. The default is `false`

## Returns

An associative array of the SMTP delivery result.

## Example

You can combine this function with our [http-bulk](https://github.com/halon-extras/http-bulk) plugin to send over the results reliably and in bulk to Postmastery.

## http-bulk configuration

```
queues:
  - id: postmastery
    path: /var/run/postmastery.jlog
    format: jsonarray
    url: https://app.postmastery.eu/v1/halonmta/<datasetID>
    min_items: 500
    max_items: 500
    max_interval: 60
    headers:
      - "Authorization: <apiKey>"
```

You need to replace `<datasetID>` and `<apiKey>` with your own values.

## HSL script

```
import { postmastery_smtp } from "modules/postmastery/postmastery.hsl";
$result = postmastery_smtp($arguments, $message);
http_bulk("postmastery", json_encode($result));
```