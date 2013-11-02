# MailToJson

Quick script to parse incoming mail and do a post with the content as JSON data.

## How to use

The work flow is quite simple. The script reads the mail mime message from STDIN, parses
the data and makes a POST call with RAW JSON to the url (passed as command line argument).

Example usage (command line):
```bash
python mailtojson.py -u http://internal.dev.newsman.ro/autoreply/handle.php
```

Example usage (postfix aliases):
```bash
mailtojson_autoreply: "|/nethosting/mailtojson/mailtojson.py -u http://internal.dev.newsman.ro/autoreply/handle.php"
```

## JSON Format

```yaml
json:
  headers:
    header_key1: value
    header_key2: value
  subject: "The email subject as utf-8 string"
  encoding: "utf-8"
  from:
    - { name: "Sender Name", email: "sender@email.com" }
  to:
    - { name: "Recipient Name", email: "recpient@email.com" }
    - { name: "Recipient Name 2", email: "recpient2@email.com" }
  cc:
    - { name: "Recipient Name", email: "recpient@email.com" }
    - { name: "Recipient Name 2", email: "recpient2@email.com" }
  parts: 
    - { content_type: "text/plain", content: "body of this part" }
    - { content_type: "text/html", content: "body of this part" }
  attachments:
    - { filename": "invoice.pdf", content_type: "application/pdf", content: "base64 of binary data" }
    - { filename": "invoice2.pdf", content_type: "application/pdf", content: "base64 of binary data" }
```

### Handling JSON data in PHP

Here is a quick example of how to parse the JSON data posted by the mail to json script:

```php
<?php

$json_str  = file_get_contents("php://input");
$json_data = json_decode($json_str, true);

var_dump($json_data);
?>
```

# License

This code is released under [MIT license](https://github.com/Newsman/MailToJson/blob/master/LICENSE) by [Newsman App - Smart Email Service Provider](http://www.newsmanapp.com).
