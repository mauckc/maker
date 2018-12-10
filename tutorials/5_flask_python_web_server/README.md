# Flask Hello world!

Launch a web-server using flask


## Flask Homepage

[Flask Homepage](flask.pocoo.org)

### Easy to setup
```python

from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
```

### Easy to run

```shell
$ pip install Flask
$ FLASK_APP=hello.py flask run
 * Running on http://localhost:5000/
 ```
 
 ### Easy to access
 
 Navigate to your browser and enter http://localhost:5000/ as your URL to view your webpage.
