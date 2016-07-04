https://travis-ci.org/

* log in to Travis CI, connect account with Github
* find appropriate repository in the list and flip the switch
* add the following to config.py:
```
class TravisConfig(object):
    SQLALCHEMY_DATABASE_URI = "postgresql://localhost:5432/blogful-test"
    DEBUG = False
    SECRET_KEY = "Not secret"
```
* create file `.travis.yml` in root of project folder with the following contents:
```
language: python
python:
    - "3.4"
install: python3 -m pip install -r requirements.txt
env:
    - CONFIG_PATH=blog.config.TravisConfig
before_script:
    - psql -c 'create database "blogful-test";' -U postgres
script:
    - PYTHONPATH=. python3 tests/test_filters.py
    - PYTHONPATH=. python3 tests/test_views_integration.py
    - PYTHONPATH=. python3 tests/test_views_acceptance.py
    # Add any other tests here
```