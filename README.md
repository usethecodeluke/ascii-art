# ascii-art
Flask JSON API to convert images into ASCII art

Installing dependancies
------------------------------------------------

1. Install `python-pip` from your package manager
    * Debian: `sudo apt-get install python-pip`
    * Redhat: `sudo yum install python-pip`
    * OSX: `brew install python-pip`
2. Install virtualenv: `pip install virtualenv`
3. Initialize a virtualenv in toplevel ascii-art directory:
`virtualenv ./`
4. Activate the virtualenv
`source bin/activate`
5. Install package requirements 
`pip install -r requirements.txt`

Running Test Server
------------------------------------------------
1. Setup Flask environment variables:
    * `export FLASK_APP=ascii_art/ascii_art.py`
2. Move to directory containing this README
3. Start virtualenv: `source bin/activate`
4. Run Flask server: `flask run`

The server should now be running on `localhost:5000`

Running Production Server
------------------------------------------------

We can use Gunicorn to run multiple workers of our app.
For example:

`gunicorn -w 4 -b 127.0.0.1:5000 ascii_art.ascii_art:app`

Of course the number of workers and port binding can be configured
as desired.

Using the API
------------------------------------------------

This simple API has only one endpoint.

Endpoint | Methods | Accepts                                   | 200 JSON
---------|---------|-------------------------------------------|--------------
/artify  | POST    | mutlipart file upload with one key: "file" | '{"art": "ascii-art-string"}'

###Return Values:

*200*: On success a JSON object with one key `art` whose value is the ASCII art
string is returned. The ascii string is scaled to a maximum width or height of 256
characters, while the aspect ratio is maintained. The string is newline
seperated, where each line represents one row of the rasterized image from top
to bottom.

*400*: If there was a problem with the uploaded file, a BAD REQUEST status is
returned and an error message is passed in the response body.

###Example (requires cURL):

`curl -X POST --form file=@/location/of/image/file.png localhost:5000/artify`

A simple UI is provided for ASCII-ifying and viewing pleasure. Simply navigate
to `localhost:5000/` in your browser to use.

###Example using Python requests:

```python
>>> import requests
>>> url = 'http://localhost:5000/artify'
>>> image = open('tests/fixtures/image', 'rb')
>>> response = requests.post(url, files={'file': image})
>>> response.status_code
200
```


Running API tests
------------------------------------------------

1. Follow steps for running the test server above.
2. run `$ nosetests tests/`

Future Improvements
------------------------------------------------

* Expose image dithering output options.
* Settable ascii art output size.
* HTML output i.e <br> instead of newlines etc.
