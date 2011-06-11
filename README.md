tornado-boilerplate -- a standard layout for Tornado apps
===============================================================================

## Description

tornado-boilerplate is an attempt to set up an convention for
[Tornado](http://www.tornadoweb.org/) app layouts, to assist in writing
utilities to deploy such applications. A bit of convention can go a long way.

This app layout is the one assumed by [buedafab](https://github.com/bueda/ops).

### Related Projects

[buedafab](https://github.com/bueda/ops)
[django-boilerplate](https://github.com/bueda/django-boilerplate)
[python-webapp-etc](https://github.com/bueda/python-webapp-etc)
[comrade](https://github.com/bueda/django-comrade)

## Acknowledgements

The folks at Mozilla working on the [next version of AMO](https://github.com/jbalogh/zamboni)
were the primary inspiration for this layout.

## Directory Structure

    tornado-boilerplate/
        handlers/
            foo.py
            base.py
        lib/
        logconfig/
        media/
            css/
                vendor/
            js/
                vendor/
            images/
        requirements/
            common.txt
            dev.txt
            production.txt
        templates/
        vendor/
        environment.py  
        app.py
        settings.py

### handlers

All of your Tornado RequestHandlers go in this directory.

Everything in this directory is added to the `PYTHONPATH` when the
`environment.py` file is imported.

### lib

Python packages and modules that aren't really Tornado request handlers. These
are just regular Python classes and methods.

Everything in this directory is added to the `PYTHONPATH` when the
`environment.py` file is imported.

### logconfig

An extended version of the
[log_settings](https://github.com/jbalogh/zamboni/blob/master/log_settings.py)
module from Mozilla's [zamboni](https://github.com/jbalogh/zamboni).

This package includes an `initialize_logging` method meant to be called from the
project's `settings.py` that sets Python's logging system. The default for
server deployments is to log to syslog, and the default for solo development is
simply to log to the console. 

All of your loggers should be children of your app's root logger (defined in
`settings.py`). This works well at the top of every file that needs logging:

    import logging
    logger = logging.getLogger('five.' + __name__)

### media

A subfolder each for CSS, Javascript and images. Third-party files (e.g. the
960.gs CSS or jQuery) go in a `vendor/` subfolder to keep your own code
separate.

### requirements

pip requirements files, optionally one for each app environment. The
`common.txt` is installed in every case.             

### templates

Project-wide templates (i.e. those not belonging to any specific app in the
`handlers/` folder). The boilerplate includes a `welcome.html` template.

### vendor

Python package dependencies loaded as git submodules. pip's support for git
repositories is somewhat unreliable, and if the specific package is your own
code it can be a bit easier to debug if it's all in one place (and not off in a
virtualenv). 

At Bueda we collect general webapp helpers and views in the separate package
`comrade` and share it among all of our applications. It is included here as an
example of a Python package as a git submodule (comrade itself should't be
considered part of this boilerplate - while it might be useful, it's much less
generic).

Any directory in `vendor/` is added to the `PYTHONPATH` by `environment.py`. The
packages are *not* installed with pip, however, so if they require any
compilation (e.g. C/C++ extensions) this method will not work.

### Files

#### environment.py

Modifies the `PYTHONPATH` to allow importing from the `apps/`, `lib/` and
`vendor/` directories. This module is imported at the top of `settings.py` to
make sure it runs for both local development (using Django's built-in server)
and in production (run through mod-wsgi, gunicorn, etc.).   

#### app.py

The main Tornado application, and also a runnable file that starts the Tornado 
server.

#### settings.py

A place to collect application settings ala Django. There's undoubtedly a better
way to do this, considering all of the flak Django is taking lately for this
global configuration. For now, it works.

## Contributing

If you have improvements or bug fixes:

* Fork the repository on GitHub
* File an issue for the bug fix/feature request in GitHub
* Create a topic branch
* Push your modifications to that branch
* Send a pull request

## Authors

* [Bueda Inc.](http://www.bueda.com)
* Christopher Peplin, peplin@bueda.com, @[peplin](http://twitter.com/peplin) 
* Modifications by Stuart Marsh, http://www.beardygeek.com
