# Using Routes

## A simple calculator

In this exercise we will build a simple url-based calculator that gives us access to basic arithmetic operations. While rather trivial, this example serves to show how routes work in a web app!

### Part 1

1. We start by defining four separate routes for the four arithmetic operations that our app will support (addition, subtraction, multiplication, division). We only walk through one of these routes and leave the rest to you!

2. Start off by making a new app.py in a folder. **Make sure that you have your virtualenv activated!**. Import the flask library and create your flask app.
  ```python
  from flask import Flask

  app = Flask(__name__)
  ```

2. We need to add routes to our app now! The addition route will require both a static component to tell the server which operation we want to do (`add`) and which inputs we want to add together. Try adding the following lines of code.
  ```python
  @app.route('/add/<x>/<y>')
  def addition(x, y):
      return x + y
  ```

3. Now run your app and try using that route! Remember that you can run your app, when you are in your virtualenv, by simply running the following two commands:
  ```shell
  $ export FLASK_APP=app.py
  $ flask run
    * Serving Flask app "app"
    * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
  ```

4. Open a web browser and go to `localhost:5000/add/1/1`

5. Wait! We expect to see 2 as our answer but instead we see 11. This is because flask assumes that our inputs are strings and instead of adding them as integers, python concatenates the strings when you add two strings together. We can specify to flask that our inputs need to be integers with the following change to our route.
  ```python
  @app.route('/add/<int:x>/<int:y>')
  def addition(x, y):
      return x + y
  ```

6. Open up `localhost:5000/add/1/1` again.

7. You should have gotten another error! If you look at your terminal, you'll see that the last error is `TypeError: 'int' object is not callable`. Notice that now, we are no longer returning a string at the end of our function but rather an int! Flask treats non-string returns differently so the easiest solution is to just cast our output to a string. Adjust your route function as follows:
  ```python
  @app.route('/add/<int:x>/<int:y>')
  def addition(x, y):
      return str(x + y)
  ```

7. Define the rest of the routes yourself! (subtraction, multiplication, division)

8. Congrats! You should be able to use the basic arithmetic operations to get the outputs you expect.

### Part 2 (optional)
1. You may have noticed that our page doesn't display much information when displaying the result of the calculations. We are going to use a new feature of python3 to add a bit more meaning to our routes! Try changing the addition route as follows:
  ```python
  @app.route('/add/<int:x>/<int:y>')
  def add(x, y):
      return f'{x} + {y} = {x + y}'
  ```

2. If you rerun your flask server and point your browser to `localhost:5000/add/1/2` you should see `1 + 2 = 3`. Magic!

3. Implement the same descriptive results for your other routes! (This feature is known as f-strings and gives python3 the long-awaited ability to specify strings dynamically - it's actually super useful!)
