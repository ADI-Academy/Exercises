# Mongodb with Flask

## Table of Contents

## Setting up mLab
1. Go to [mlab.com](https://mlab.com) and create an account.
2. Click on the "Create New" button next to MongoDB Deployments
3. Choose a service provider (Amazon's AWS is a safe pick) and click on the sandbox (free) option. Click continue.
4. Select US East. Click continue.
5. Choose a name for your database. Click continue.
6. Review your options and then click Submit Order. If all goes well, then you've properly setup your mongodb instance!
7. In order to use your database, you need to create an authenticated user to access that database. To do so, click on your database on the dashboard and click on the User Tab.
8. Click on "Add database user" and create a new database user.

## Setting up Mongodb
1. Install ```flask-mongoengine``` in your virtualenv. **REMEMBER TO ACTIVATE YOUR VIRTUALENV**
  ```
  $ pip install flask-mongoengine
  ```
2. Just as with [```flask-bootstrap```](../forms), we need to import our mongodb libraries and initialize our app with the MongoEngine instance. Import ```flask-mongoengine``` at the top of your ```app.py```.
  ```python
  from flask_mongoengine import MongoEngine
  ```
3. Create and initialize your MongoEngine instance
  ```python
  app = Flask(__name__)
  ...

  db = MongoEngine()
  db.init_app(app)
  ```
4. The MongoEngine instance will automatically connect to your local mongodb instance running on port ```27017```. (If you don't know what all this means, just ignore it). In our demo and here, we will be showing how to connect with our mlab mongodb instance.
5. We will need to identify five pieces of information in order to connect to your database. If you click on your database from the mlab dashboard you'll see instructions for connecting via the MongoDB URI. You should see something that looks like
```
mongodb://<dbuser>:<dbpassword>@ds125016.mlab.com:25016/academytest
```
6. The Host: The first item we need is the host name. In our example, this is the URI that appears after the @ symbol: ```ds125016.mlab.com```.
7. The Port: There will be a specific port onto which you can connect to your database. In our example, this is the sequence of numbers that appears after the colon at the end of the host name: ```25016```.
8. The DB: We need the database name. This is simply the name that you gave your database when you created it. You can also see that it is the last part of your mlab URI: ```academytest```.
9. The Username and Password: You should know the username and the password that you created for this database earlier when setting up mlab. Keep this in mind.
10. We now have all of the information to connect to our mlab instance! We need to edit an variable in ```app.config``` which we have seen many times now. To tell Flask how to connect, add the following lines of code to your file and replace the variables with the actual names that you gathered in the previous step:
  ```python
  app.config['MONGODB_SETTINGS'] = {
      'db': NAME_OF_YOUR_DATABASE,
      'host': YOUR_HOST_NAME,
      'port': YOUR_PORT_NUMBER,
      'username': YOUR_USERNAME,
      'password': YOUR_PASSWORD
  }
  ```
  **HINT:** With the exception of the port field, which must be a number, all of the other fields should be strings.
11. You should now be able to connect to your database!

## Setting up your Schema
You (should) have seen in lecture how to setup a database schema using MongoEngine. To reiterate, a schema is a structure that we impose on a databse in order to organize the information that we store. For example, if our database stores book listings, we may have a schema that includes the title of the book, the author of the book, and the ISBN id. In this tutorial, we will be setting up a User schema.

4. Import the db schema fields from [```mongoengine```](http://mongoengine.org/). **NOTE:** This library is different from the ```flask-mongoengine``` library that we just imported earlier. ```mongoengine``` is the actual ODM (Object Document Mapper) that can convert between python objects and their database representations.
  ```python
  from mongoengine import StringField, EmailField
  ```
2. In your ```app.py``` file, create a User schema class. Instead of using our example, you can choose to define the schema however you want! It is import to think about how you want to design your User object (yay for 1007!) and which fields you want to include for your specific web app. The schemas will be defined by the ```StringField```, ```EmailField```, or ```*Field``` (where * just stands for [some other type of field](http://docs.mongoengine.org/apireference.html#fields). Feel free to experiment or ask TAs for help in adding more fields to your schema). Remember that the structure of our schema class will look something like the following:
  ```python
  class User(db.Document):
      field1 = StringField(key1='something', key2='something', ...)
      ...

  ```
3. Once we have our schema defined we can actually put it to use! If you have been following along with the previous lectures, you should have a login route already defined that ask users for a username, an email, and their password. We are going to simply rename this route to signup to better reflect its role. Simply change the route as below while filling out the necessary information.
  ```python
  @app.route('/signup', methods=['GET', 'POST'])
  def signup():
      ...
      return render_template('signup.html', form=form, ...)
  ```
  Then change the .html file from ```login.html``` to ```signup.html```.
4. As was shown in lecture, we can create a new database object from our collect form information and save this new user in our database! This complicated process is made extremely easy with our schemas. Add something along the lines of the following.
  ```python
  if form.validate_on_submit():
      user = User(username=form.username.data, email=form.email.data)
      user.save()
      ...
      return redirect(url_for('signup'))

  return render_template(...)
  ```
  The ```user.save()``` line is what will save your object into your database!
5. If you run your app and don't encounter any errors then you should be able to add users to your database! Submit a test entry for form and check on mlab to see if an entry was added! You can look more in depth at the entry to see exactly what mongodb is storing when you are storing your users.

## Extra Credit Profile Page! (lol not really)
One cool thing we can do is to create a custom linked profile page (as we saw for books) for our users. To do so, we need to create a custom route that takes a username in the url and finds the corresponding user in the database. As opposed to direct instructions, here are some hints as to how you can implement that.

**Hint 1:** Remember that we can use a route to take in input from the url like this:
```python
@app.route('<user>')
def get_user(user):
    '''Your logic goes here'''
    ...
```
**Hint 2:** Remember that we can find an user in our database with the following line code and by substituing the appropriate parameters:
```python
User.objects(PARAM=YOUR_PARAM).first()
```

**Hint 3:** You will need to create a new ```.html``` page that can display user information somehow. Remember that in Jinja we have the the following constructs:

Access Variables: ```{{ some_variable_passed_to_render_template }}```

Control Flow: ```{% if <cond> %} DO_STUFF {% else %} DO_STUFF {% endif %}```
