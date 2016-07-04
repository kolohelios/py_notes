# CONFIG_PATH
add a testing config to config.py

PYTHONPATH=.
set the current path as PYTHONPATH

create a client for Flask views using app.test_client(), then call client.get(url)

we use integration testing because sometimes you cannot provide enough isolation to use unit testing

* unit testing - test a single method
* integration testing - test an endpoint where dependencies are not being isolated
* acceptance testing - unlike unit and integration testing, which are developer-focused, acceptance testing is is user-focused based on user behavior

running with the **multiprocess** module allows one to start and run other code simultaneously within other scripts
* uses start and terminate
* implements concurrency
* uses a "Process" object
* can't call app.run as usual because it will be blocking; it gets called as a separate process

Splinter is a Python-friendly wrapper for Selenium:
for example, we can refer to `Browser.fill(field_name, value)` where `field_name` is the name attribute of the input element

*Nose* wraps around Python's UnitTest framework, so we can point to a single directory and it finds tests
we use the `nosetest` command to run tests (no need to set PYTHONPATH)

