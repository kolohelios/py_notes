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

super blocks: {{ super() }} - renders the parent's template block before the child's template block (and super() goes in the child template)

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

