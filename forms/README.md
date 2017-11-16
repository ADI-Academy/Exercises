# Adding forms to your website

User interaction is one of the most important features of a websites and forms are often the most common and effective way to receive user response.

### Table of Contents
1. [Flask-Bootstrap](#flask-bootstrap)
2. [Forms](#wtforms)


## Flask-Bootstrap
If you've been following along, you realized that we've been using the bootstrap library to render our CSS and styles. However, there is a more efficient way of using bootstrap that also adds a lot of useful functionality.

1. Download [flask-bootstrap](https://pythonhosted.org/Flask-Bootstrap/) by using the following command in your terminal. *Make sure that your virtual environment is activated!!*
  ```
  $ pip install flask-bootstrap
  ```

2. First off, we need to initialize flask-bootstrap in our app by editing our ```app.py```. First import it:
  ```python
  from flask import Flask, ...
  from flask_bootstrap import Bootstrap

  ```
3. We then need to initialize flask-boostrap for our current app by adding the last two lines:
  ```python
  app = Flask(__name__)
  ...
  bootstrap = Bootstrap()
  bootstrap.init_app(app)
  ```

4. We now have Bootstrap functionality! In order to see the results, we will need to edit our ```base.html``` template file (in the templates folder). Add the following line to the top of the file:
  ```html
  {% extends "bootstrap/base.html" %}
  ```
  This will automatically include all the necessary tags for your html file (including the ```<html>...</html>``` tags). Delete the opening and closing html tags now.

5. We will also replace the ```<head>...</head>``` portion with the specially generated bootstrap templating code.
    ```html
    {% block head %}
    {{ super() }}
    <!-- CUSTOM LINKS TO CSS AND JAVASCRIPT GO HERE -->
    {% endblock %}
    ```
6. We can also add a special block tag aronud our navigation bar.
  ```html
  {% block navbar %}
    <div class="navbar navbar-inverse" role="navigation">
      ...
    </div>
  {% endblock %}
  ```
  Refer to the example code included in this folder to make sure that your ```base.html``` file matches.

7. If you run your web app now, you should be able to see the exact same results as before! It looks like nothing has changed, but we've just abstracted away a lot of the manual import of bootstrap files and we'll see later in this lesson the other uses that this gives us.


## WTForms
Forms are an extremely important part of any web application. During lecture, we have seen an example of how to use raw forms, but with flask we have a much more efficient and powerful method of using forms.

1. Download WTForms by using the following command. *Again, make sure that you are in your virtual environment*
  ```
  $ pip install flask-wtf
  ```
  This command should install both ```flask-wtf``` and ```WTForms```.

2. Let's create our first form! We'll create a simple login form that asks for the username, password, email and name. (Feel free to add other functionality as well) Create a new file and name it ```form.py```. Make sure that it exists in the same directory as your app.py. We do this to separate our code and make one part independent from other parts.

3. Import the necessary form requirements - we'll go over what these each mean as we build our custom form.
  ```python
  from flask_wtf import FlaskForm
  from wtforms import StringField, PasswordField, SubmitField
  ```

4. We need to build a class (kudos to 1004!) that describes our form. This is a lot simpler than you might imagine.
  ```python
  class LoginForm(FlaskForm):
      email = StringField('Email')
      username = StringField('Username')
      password = PasswordField('Password')
      submit = SubmitField('Register')
  ```

5. Now that we have our form, we need to have a way to process it. Since all of our logic and processing will be done in ```app.py``` we need some way to link the form with the app. We can do this by importing our ```LoginForm``` from ```form.py```. Add the following line at the top of your ```app.py``` file.
  ```python
  from flask import Flask, ...
  from form import LoginForm
  ```

6. We've arrived at the complicated part. Now that we have our LoginForm object in our ```app.py```, how should we process it? There are many complicated things that you can do with forms (saving a user, making a transaction, etc.) but for now, we'll just print out the username back to the user to demonstrate how forms work. We'll be working with the ```login``` route in ```app.py``` that we defined in a previous lecture.
  ```python
  @app.route('/login')
  def login():
      username = None
      form = LoginForm()
      if form.validate_on_submit():
          username = form.username.data
          print username
      return render_template('login.html', form=form, name=username)
  ```

7. You will also need to set a secret key in order to enable [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) protection. Add the following line to your ```app.py```:
  ```python
  app = Flask(__name__)
  ...
  app.config['SECRET_KEY'] = 'SOME_STRING_THATS_SECRET'
  ```
  Ideally, we would put the SECRET_KEY as an environment variable in our system, but we'll just keep it hardecoded in the app for now.

8. From our basic (and actually incomplete) form processing, we now want to be able to display our form to see. Luckily, there is an extremely easy way to do this using flask-bootstrap and flask-forms. Navigate to the ```base.html``` and insert the following line at the top:
  ```html
  {% import "bootstrap/wtf.html" as wtf %}
  ```
  This will give us functionality to use wtforms in html.

9. Now go to your ```login.html``` that is being rendered by the ```login``` route. We only need to add one line to display our new form!
  ```html
  {% extends 'base.html' %}
  ...
  {% block content %}
  <div class="col-md-4">
    {{ wtf.quick_form(form) }}
  </div>
  {% endblock %}

  ```
  We will also add a little tag that will display the name variable if its defined.
  ```html
  {% block content %}
  {% if name %}
    <h1>Hello {{ name }}</h1>
  {% endif %}
  ...
  {% endblock %}
  ```
10. If all goes well, then you should be able to see your generated form as if by magic!! However there's a problem - if you try to submit you'll run into errors.

11. First, you'll see a Method Not Allowed error. Our ```login``` route only accepts GET requests right now but our forms will be submitting data via a POST request. (We are asking for a password so we need security!). To fix this, change the definition of your login route.
  ```python
  @app.route('login', methods=['GET', 'POST'])
  def login():
      ...
  ```

12. Now if you try to submit, it should go through! If you check the terminal output, you can also see the username printed out so everything seems to be going fine. HOWEVER, you should also notice that the values that we input to our values didn't change.
