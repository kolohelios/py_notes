#How to create a Flask app:#

1. `from Flask import Flask`
2. create the Flask app instance with `app = Flask(__name__)`
3. views are wrapped in the `@app.route('/someroute')` decorator function
4. returning a value from the function wrapped by the decorator will be Flask's response (so far, this has only been strings, either plain text or HTML – plain text gets wrapped by Flask into the appropriate HTML)
5. parameterized path routes can be created by including angle braces inside the route, such as `@app.route('/library/books/<book_name>')`, and the value will be available to the wrapped function as an argument

#Templating with Jinja2#

templates go in /templates
import from flask render_template
in the route's function, 
`return render_template('template.html', my_string='something', my_list=[0,1,2])`

{ ... } is used for expression or logic (like for loops); {{ ... }} is used for actually outputting the result of expressions or variables to the HTTP response

Jinja only supports *if* and *for* statements; endif and endfor take the place of whitespace for termination

inheritance works through {% extends %} and {% block %}

super blocks: {{ super() }} - renders the parent's template block (before) the child's template block (and super() goes in the child template); allows code from the parent to be used in multiple children templates

macros are included at the beginning of a base template like this:
{% from "macros.html" import nav_link with context %}

and are defined with the following syntax:
{% macro nav_link(endpoint, name) %}
{% if request.endpoint.endswith(endpoint) %}
    <a href="{{ url_for(endpoint) }}" class="active">{{name}}</a>
{% else %}
    <a href="{{ url_for(endpoint) }}">{{name}}</a>
{% endif %}
{% endmacro %}

and used like this:
{{ nav_link('home', 'Home') }}

filters:
used like in Angular
{{ num | round }} or {{ list | join(', ') }}

here's a custom filter that would be defined in the main python file:
@app.template_filter()
def datetimefilter(value, format='%Y/%m/%d %H:%M'):
    """Convert a datetime to a different format."""
    return value.strftime(format)
    
the name of the filter will be the same as the function's name unless a different name is passed as an argument to the decorator, like: @app.template_filter('formatdate')

the custom filter is used like any of the standard filters:
<h4>Current date/time: {{ current_time | datetimefilter }}</h4>

and with this example, we have to make sure we define current_time and pass it to the render_template call, like with
current_time = datetime.datetime.now()

from . import views
*imports additional modules from the current package only - shorthand for blog.views, for example*

from flask.ext.script import Manager
http://flask-script.readthedocs.io/en/latest/
*Flask-Script defines tasks that help manage ones application*

@manager.command
^^^^ decorator for adding a command to a Flask-Script

Flask.config.from_object
It takes a string containing the path to a file, dictionary or class, and uses the variables in the corresponding object to populate the config object
really: it allows us to have discrete configurations for a single app
http://flask.pocoo.org/docs/0.11/config/

Jinga - use_for:
You can use the url_for function to generate the href tag. For example <link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}"> would link to the file static/css/main.css.

Flask.template_filter
decorator for pipe filters

Jinja safe filter allows unescaped HTML and should be used judiciously

UserMixin class from Flask-Login allows us to use methods that are related to users

Flask-Login's user_loader decorator attaches the user to a session after loading them from the database

the flash function caches messages to display at the next page load - it's a queue, basically

@login_required decorator will provide the requested resource only if the user is logged in

#Flask migrations - database migrations for the win!#
to add to manage.py
```
from flask.ext.migrate import Migrate, MigrateCommand
from blog.database import Base

class DB(object):
    def __init__(self, metadata):
        self.metadata = metadata

migrate = Migrate(app, DB(Base.metadata))
manager.add_command('db', MigrateCommand)
```

then python manage.py db init to initialize the migration scripts
make changes to models
then run python manage.py db migrate
check the resulting script in /migrations/versions
if it looks good, run python manage.py db upgrade

if things get screwed up, run python manage.py db downgrade

session_transaction stores the HTTP ession variable so we can add and remove items from the session