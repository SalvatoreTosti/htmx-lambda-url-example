# HTMX AWS Lambda URL Example

### Configuring the Lambda
1. Create a new lambda with the following
* Function name - HTMX-example-lambda
* Runtime - Python 3.9
* Architecture - x86_64

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
4. Under the `Configuration` tab select `Function URL` from the left navigation panel
5. Then click the `Create function URL` button
6. Create a Function URL with the following properties
* Auth type - `NONE`
* Configure cross-origin resource sharing (CORS) - Enabled
* Allow origin - `*` (allow all)
* Add the following values under “Expose headers”
  - access-control-allow-origin
  - access-control-allow-methods
  - access-control-allow-headers
* Add the following values under “Allow headers”
  - hx-current-url
  - hx-request
* Select the following values under “Allow methods”
  - POST
7. Click “Save"
8. In the main menu, look for the `Function URL` and open it in a new tab, you should see:
  ```
    Hello {name} from a lambda!
    Your message: {message}
  ```
  
### Testing Locally
1. Edit the `index.html` file and set the `form.hx-post` attribute to your lambda’s URL
2. Drag `index.html` to your browser
3. Enter values into the `Name` and `Message` fields and click “Submit”
4. Observe that the form is replaced by:
```
Hello <your name> from a lambda!
Your message: <your message>
```

