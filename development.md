DEVELOPMENT
===

* [GENERAL TOOLS & PACKAGES](#general)
* [WEB DEVELOPMENT](#web)
    - [AngularJS](#angular)
    - [Ruby on Rails](#ruby)
* [TRAVIS](#travis)

---

<a name="general"/>
# General Tools & Packages

### Xcode and Command line Tools: 
I needed to subscribe first as a Mac developer. 
___Caution___: after installing big updates of Xcode it is necessary to accept the terms or it will not compile the software. You can do this through the command line running this `sudo xcrun cc`

### Homebrew: 

it requires to install Xcode first. And using brew I installed, for example:

   * RUBY: brew install ruby
   * GO programming language: brew install go
   * Github for Mac
   * Vagrant: to manage virtual Linux from the command line. It uses VirtualBox (already installed) in the computer.
   * Sass: sudo gem install sass
   * Hydo: HTML5 editor

### NPM, the official package manager for Node.js

_This is an special section giving its importance for frontend development._

Ideally, npm should be installed and run locally. The problem is that make it work is not trivial. I mess everything up by installing node again, and that messed everything up. So it is necessary to be extraordinarily careful with `node`, `npm`, `bower`, `yeoman`, and company. Dependencies can be a big headache.

###### INSTALL EVERYTHING
* `brew doctor`
* `brew prune`
* `brew install node`
* `npm install -g yo grunt-cli bower`
* `sudo chown -R ``whoami`` ~/.npm`
* `sudo chown -R ``whoami`` /usr/local/lib/node_modules`
* `npm update object-keys`
* `npm install object-keys`
* `npm install -g bower-config`
* `npm update -g`
* `npm install -g chalk`

###### Specific problem with npm and installations

Right after a big update of `Xcode and Command line Tools`, npm installations started failing again. The problem seemed to be that I had to accept the terms and conditions of `xcode`, and otherwise, the compiler was not working and, therefore, generating problems. After accepting the terms:

* Delete `./npm`
* `npm install -g yo grunt-cli bower`
* `sudo chown -R ``whoami`` ~/.npm`
* `sudo chown -R ``whoami`` /usr/local/lib/node_modules`
* `npm update object-keys`
* `npm install object-keys`
* `npm install -g bower-config`
* `npm update -g`
* `npm install -g chalk`

And after testing generator-angular, everything was back to normality.

__Testing__

* `npm install -g generator-webapp`
* `mkdir testfolder && cd $_`
* `yo webapp`
* `npm install -g generator-angular`
* `mkdir testfolder && cd $_`
* `yo angular`


###### UNINSTALLED IT

PROBLEMS here: I decided to install node from the web and I screwed it up> I did it without thinking just following some tutorial and I didn't realize what I was doing. This generated a big mess in my Mac, so I have so start all over again following advices. Steps to uninstall everything:

* `lsbom -f -l -s -pf /var/db/receipts/org.nodejs.pkg.bom | while read f; do  sudo rm /usr/local/${f}; done`
* `sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*`
* `rm -rf /Users/[homedir]/.npm`


<a name="web"/>
# WEB DEVELOPMENT

<a name="angular">
## AngularJS
Check the [developers corner](https://github.com/dictyBase/frontpage-dictybase/blob/develop/documentation/developers-corner.md) at the [frontpage-dictybase](https://github.com/dictyBase/frontpage-dictybase) project.

<a name="ruby"/>
## Ruby on Rails

#### Manuals/Tutorials/Videos

* [What is Ruby on Rails?](http://railsapps.github.io/what-is-ruby-rails.html)
* [Installing Ruby on Rails on Mac OS X Mavericks](http://railsapps.github.io/installrubyonrails-mac.html): review the advices in this article just in case.

#### Ruby installation, configuration, management

***ALWAYS Manage Ruby using rbenv***. 
Follow their [instructions](https://github.com/sstephenson/rbenv). Check the README file on the github project `dictyconference` [develop branch](https://github.com/dictyBase/dictyconference/blob/develop/README.md) for more details.

___Important___: if you install a new ruby version (e.g. `rbenv install 1.9.3-p429`), then you need to install again rails doing `gem install rails` and them install again `bundle install`.

Useful `rbenv` commands:

* `rbenv install -l`

Also install `bundler`, which let you use `bundle`, which takes care of installing all the dependencies.

* `gem install bundler`

It is also necessary to use `rake` to manage the databases (and other stuff) for a ruby app. `Rake` is a Make-like program implemented in Ruby. For example, to test (staging) the `dictyconference`, I followed these steps

```
cp database.yml.sample database.yml
rake -T # list all the options of rake
rake db:create
rake db:migrate
```

## Some useful TOOLS

* [Webpack](http://webpack.github.io/docs/what-is-webpack.html): `npm install -g webpack`

<a name="travis"></a>
# Travis CI
Travis CI is a hosted continuous integration service. It is integrated with GitHub and offers first class support for a lot of different languages. These are the configuration details for different projects:

<a name="angular"></a>
#### Angular on Travis

For illustration purposes, this description will be based on the dictyBase project [dictyHeaderFooter-Angular-Directive](https://github.com/dictyBase/dictyHeaderFooter-Angular-Directive).

In this project the standard yeoman `generator-angular` is used to build the angular directives for the footer and header of the next dictyBase. The default task manager is `Grunt` for the yeoman project. In order to make it work on PhantomJS and Firefox, this is the specifications that has to be included in the `.travis.yml` configuration file:

* `.travis.yml`

        before_script:
          - npm install -g grunt-cli
          - npm install -g bower
          - npm install -g karma
          // PhantomJS
          - npm install karma-phantomjs-launcher -save-dev
          - bower install
          // Next, for Firefox to work on Travis:
          - "export DISPLAY=:99.0"
          - "sh -e /etc/init.d/xvfb start"


