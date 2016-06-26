Heroku toolbelt

$ heroku login

$ heroku create

$ pip install gunicorn

$ echo 'web: gunicorn hello_world:app --log-file=-' > Procfile

Finally, you need to specify your Python version. Check their list of supported Python runtimes and choose the latest supported Python 3 version; then declare that as your application's preferred runtime:
$ echo python-3.7.5 > runtime.txt
Finally, you need to specify your Python version. Check their list of supported Python runtimes and choose the latest supported Python 3 version; then declare that as your application's preferred runtime:

$ echo python-3.7.5 > runtime.txt

before deploying, test by using:
$ heroku local
(then use the preview tool or echo $C9_HOSTNAME to get the URL)

kill the heroku process (ctrl-c)

$ pip freeze > requirements.txt
(outputs pip requirements and their rev levels)

commit to git

$ git push heroku master # Now you can push to Heroku

You should see a bunch of notifications in the terminal. After it's done, you make sure you have a dyno running to serve the app:

$ heroku ps:scale web=1
Scaling web processes... done, now running 1

$ heroku open
(to open a browser to take a look- it may not work, so check the dashboard for the app's URL)

Heroku Python guide:
https://devcenter.heroku.com/articles/getting-started-with-python#introduction

Other stuff that one can do:
* You can run a command, typically scripts and applications that are part of your app, in a one-off dyno using the heroku run command.
* You can view a stream of your application logs with heroku logs --tail. Use Ctrl-c to exit.
* There are a ton of add-ons for Heroku that can help you manage your app: Heroku Add-ons.
* One of those add-ons is a free Postgres database, which will be helpful in any of your capstone projects that require a database. (There are a bunch of other databases available too.)
* If your app uses environment variables (for example, a `DB_USER` and `DB_PASS` in the SQL configuration), you can set the Heroku environment variables with heroku config:set VAR_NAME=value.