# HTMX lambda URL example

## Configuring the Lambda
1. Create a new lambda with the following
* Function name - HTMX-example-lambda
* Runtime - Python 3.9
* Architecture x86_64

2. Under the `Code` tab, locate the `lambda_function.py` file and edit it to be as follows:
```python
import base64
from urllib.parse import parse_qs

def lambda_handler(event, context):
    body_dec = base64.b64decode(event.get("body","")).decode('UTF-8')
    parsed_body = parse_qs(body_dec)
    name = parsed_body.get('name')
    message = parsed_body.get('message')
    return {
        'headers': {'content-type': 'text/html'},
        'statusCode': 200,
        'body': f'''
          <h2 class="text-center text-xl my-5">Hello {name} from a lambda!</h2>
          <h3 class="text-center text-xl my-5">Your message: {message}</h3>
          '''
    }
```
3. Click the ‘Deploy’ button
