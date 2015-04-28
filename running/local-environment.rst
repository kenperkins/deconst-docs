How to setup a local deconst environment
========================================

Deconst uses homebrew on OSX for dependencies. As such, all of the work in this setup guide is based on a healthy homebrew installation, or installing it fresh.

Update brew database or install homebrew
----------------------------------------

First, we need to setup (or update) homebrew on your mac. If you *already have homebrew installed*, lets just update your homebrew database\: ::

  brew update

If, however, you *don't have homebrew installed*, you can install it with a shell command. You can read more about brew installation on the `homebrew website <http://brew.sh>`_. ::

  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Once you've either installed or updated your install, you should be able to run :code:`brew --version` and see the current version of your setup. ::

  $ brew --version
  0.9.5

Install Docker
--------------

Deconst development is depending on running within docker containers. We'll use homebrew to install docker and various other docker utilities. ::

  $ brew install docker
  $ brew install docker-compose
  $ brew install boot2docker
  $ boot2docker up

One those steps are completed, we should be able to verify your docker installation is running correctly. ::

  $ docker version

  Client version: 1.6.0
  Client API version: 1.18
  Go version (client): go1.4.2
  Git commit (client): 4749651
  OS/Arch (client): darwin/amd64
  Server version: 1.6.0
  Server API version: 1.18
  Go version (server): go1.4.2
  Git commit (server): 4749651
  OS/Arch (server): linux/amd64

Install Python3
---------------

Deconst uses python3 for various preparers and other components. We will install this with homebrew as well. ::

  $ brew install python3

  $ python3 --version
  Python 3.4.3

Install Ruby
------------

Deconst uses ruby for the jekyll preparer. This is also install with homebrew. ::

  $ brew install ruby

  $ ruby -v
  ruby 2.2.2p95 (2015-04-13 revision 50295) [x86_64-darwin14]


Additional Python and Ruby Dependencies
---------------------------------------

We use **bundler** and **virtualenv** as part of our configuration for invoking components. These are installed with **pip** and **gem** respectively. ::

  $ sudo pip install virtualenv virtualenvwrapper
  $ sudo gem install bundler


Install Node Version Manager (nvm) and Node
-------------------------------------------

Deconst uses node for the assets preparer. This will be installed via nvm, which allows managing multiple node installations. ::

  $ curl https://raw.githubusercontent.com/creationix/nvm/v0.25.0/install.sh | bash
  $ nvm install 0.10
  $ nvm use 0.10

  $ node -v
  v0.10.36

Configure virtualenvwrapper
---------------------------

**virtualenvwrapper** is a set of extensions to Ian Bicking’s virtualenv tool. The extensions include wrappers for creating and deleting virtual environments and otherwise managing your development workflow. ::

  $ export WORKON_HOME=${HOME}/.venvs
  $ mkdir -p ${WORKON_HOME}
  $ source /usr/local/bin/virtualenvwrapper.sh

Optionally, you can add these steps to your shell startup script.

Fork & Clone the control and content repos
------------------------------------------

For this example, we'll be using the deconst *control-example* and *deconst-docs* repositories for configuring Deconst. You'll need to fork and clone these to your local machine.

- https://github.com/deconst/control-example
- https://github.com/deconst/deconst-docs

Clone the preparers
-------------------

In addition to the content and control repositories, you'll also need the preparers. Go ahead and clone each of these:

- https://github.com/deconst/preparer-asset
- https://github.com/deconst/preparer-sphinx
- https://github.com/deconst/preparer-jekyll

Configure Sphinx Preparer
-------------------------

Setup the sphinx preparer with your virtualenv config. ::

  $ cd preparer-sphinx
  $ mkvirtualenv deconst-preparer-sphinx --python=$(which python3)
  $ pip install -e .


Configure Jekyll Preparer
-------------------------

Setup the jekyll preparer using bundle. ::

  $ cd preparer-jekyll
  $ bundle install
  $ bundle exec rake install

Setup Cloud Environment Variables
---------------------------------

It's now time to setup your environment variables for running deconst. ::

  $ export RACKSPACE_USERNAME=asdf1234
  $ export RACKSPACE_APIKEY=xxxxxxxxxxxxxx


Run Deconst
-----------

Let's do it ::

  docker-compose up -d

