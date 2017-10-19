# Jinja Templating


## Part 1
### Creating your template directory
1. Create a new folder named `templates` in your virtualenv that you made earlier. Refer to [the Environment Setup](https://github.com/AcademyClassOf2017/EnvironmentSetup) for more info.
2. Move your home page (`.html` file) into the templates folder.

Your directory structure should look something like this:
```
academy
├── app.py
└── templates
    └── me.html
```

### Displaying your webpage
1. Open your `app.py` file in a text editor. Remember that this file is the workhorse of your Flask app – everything goes through here
2. Add a route (or change an existing one) and return your rendered webpage. Remember to use `render_template()`!

## Part 2
### Using variables
1. Modify your html file so that it can display variables. Remember the double bracket syntax to reference variables.
```html
<div> <h1> Hello {{ name }} </h1></div>
```

2. Pass a variable through the `render_template` function. It should look something like this:
```python
return render_template("me.html", name=name)
```
Start your web app (`python app.py`) and point your webpage to the correct address and you should see your webpage.

3. Let's try something more complicated! Create a list in your `app.py` of the names of your group member and pass the entire list to your template.

4. Use the `{% for x in X %}` syntax to print out everyones name in an html list.


### Control Flow
1. Create a [python dictionary](https://docs.python.org/2/library/stdtypes.html#dict) with they key as the names of your group members and the values as the heights in inches. **Example:**
```python
my_friends = {'jonathan': 68, 'cesar': 62, 'nick': 100}
```

2. Just like in the previous step, we'll iterate through all the entries in your dictionary and print out their names. (HINT: you can use `for key in my_friends` to do this). However, this time we will add an if statement in each loop to check if their height is over 65 inches (or any other arbitrary number; I promise I have nothing against people above or below 65 inches). If the height is less than the threshold, [bold](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/b) their names and if under, [italicize](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/i) their names!  Remember the following syntax:
```html
{% if CONDITION1 %}
...
{% elif CONDITION2 %}
...
{% else %}
```

### Advanced
1. If you know the basics of object oriented programming and classes in python, create a `User` class for the previous examples and add some more attributes (age, major, hours slept this week, etc.). Generate a profile for each member by passing the list of your User objects to your template! (You might want to use CSS to make it look nice)

## Part 3

### Template Inheritance
1. Create a new html file name `base.html` or some other easy to remember name. Copy and paste the following code into that file. This will form the base of your html file!
```html
<html>
<head>
<link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
</head>

<body>
    <div class="navbar navbar-inverse" role="navigation">
        <div class="container">
            <div class="navbar_header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="sr-only">Toggle Navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>

            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li><Home</li>
                </ul>
            </div>
        </div>
    </div>

{% block content %}
{% endblock %}

</body>
</html>
```
For all future html documents, you can extend this document by adding
```
{% extends "base.html" %}
```
Remember you can add all of your content in the future within a pair of block tags
```
{% block content %}
YOUR PAGE CONTENT HERE
{% endblock %}
```
2. Create (or copy) your personal webpage and make sure that it extends your `base.html`.
