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

3. Let's try something more complicated! Create a list in your `app.py` of the names of your group member and pass the entire list to your template.

4. Use the `{% for x in X %}` syntax to print out everyones name in an html list.


### Control Flow
1. Create a second list with your group members ages (or any other metric honestly).

2. Create an if statement using the
```html
{% if CONDITION1 %}
...
{% elif CONDITION2 %}
...
{% else %}
```
in each of your for loops to check

### Advanced
1. If you know the basics of object oriented programming and classes in python, create a `User` class for the previous examples

## Part 3

### Template Inheritance
1. Create a new html file name `base.html` or some other easy to remember name.
2.
