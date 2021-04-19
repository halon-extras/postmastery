## postmastery_smtp(arguments, message)
This function converts a SMTP delivery result into a format that is compatible with Postmastery's Delivery Analytics product.

**Params**

- The [pre-defined](https://docs.halon.io/hsl/postdelivery.html#v-z1) `$arguments` variable
- The [pre-defined](https://docs.halon.io/hsl/postdelivery.html#v-m1) `$message` variable

**Returns**

An associative array of the SMTP delivery result.

**Example**

You can combine this function with our [httpbulk](https://github.com/halon-extras/httpbulk) plugin to send over the results reliably and in bulk to Postmastery.

***httpbulk configuration***

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

***HSL script***

```
import { postmastery_smtp } from "modules/postmastery/postmastery.hsl";
$result = postmastery_smtp($arguments, $message);
httpbulk("postmastery", json_encode($result));
```