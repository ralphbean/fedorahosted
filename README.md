# Fedora Hosted Processor

This application stores, manages, and processes Fedora Hosted requests.

# Project Goals

## Web Interface

* Provide an easy interface for any person to request a project on Fedora
  Hosted, and store the requests in a database.
* Be awesome. Don't suck.

## CLI Interface

* Allow administrators to easily process new hosted requests.
* Allow users to easily create hosted requests (alternative to the web interface).
* Provide a noop mode to see what would happen without actually doing anything.
* Be awesome. Don't suck.

### Workflow

* Admin starts processing the request (`./fedorahosted-process.py -p 1`)
* Script asks for admin's FAS username/password.
* Script creates the new FAS group.
* Script runs all the commands needed on hostedXX.
* Script tells the web app to check with FAS and see if the new group exists.
  * This prevents having to send FAS auth info to the web app.
  * If the group exists, the web app sets HostedRequest.completed = true

# Dependencies (Fedora packages)

* python-fedora
* python-flask
* python-flask-sqlalchemy
* python-flask-wtf
* python-sqlalchemy
* python-wtforms
* python-fedmsg
* python-pep8 (if you plan on hacking, not needed to deploy)

# Fedmsg-specific

You probably don't want to merge this into master yet since python-fedmsg hasn't made it into fedora yet.

If you want to try it out though, merge it into an experimental "fedmsg" branch.

### Install Dependencies

```
sudo yum install python-virtualenv python-setuptools python-psutil openssl-devel python-devel python-zmq python-twisted
virtualenv --system-site-packages fenv
source fenv/bin/activate
pip install -Iv WebOb==1.0.8
pip install fedmsg
```

### Run `fedmsg-tail` and the unit tests at the same time

In two different terminals, run:

```
source fenv/bin/activate
fedmsg-tail --really-pretty
```

```
source fenv/bin/activate
python webapp-tests.py
```

# Deploying

To set up the app, copy the included `fedorahosted_config.py.dist` to
`fedorahosted_config.py` and edit its values appropriately.

Then, look to [Flask's Documentation](http://flask.pocoo.org/docs/deploying/)
for figuring out the best way to deploy. In Fedora Infrastructure, we will
likely use mod_wsgi, simply for the fact that we already have infrastructure
built up around it, and know how to support it.

# Hacking/Contributing

Please have read and signed the
[Fedora Project Contributor Agreement](http://da.gd/fpca) before sending
patches.

Please follow PEP 8 and ensure all unit tests pass.

A good `.git/hooks/pre-commit` hook is as follows (and make sure the hook is
chmod +x) though it does depend on the package: `python-pep8`:

`pep8 *.py && python webapp-tests.py`

Above all, have fun!
  
# License

GPLv2+
