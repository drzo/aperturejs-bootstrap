# Aperture JS bootstrap

The Aperture JS bootstrap project is a simple full-stack starting point for creating single-page applications using [Aperture JS](https://github.com/oculusinfo/aperturejs). It separates development of the html/js/css client (using client development best practices) from that of the Java server (with Java best practices). This client-server code separation was inspired by [Making Maven Grunt](http://addyosmani.com/blog/making-maven-grunt/) by [Addy Osmani](http://addyosmani.com/) and the [JHipster yeoman generator](http://jhipster.github.io/).

## Design

The client-side development environment uses [bower](http://bower.io/) for client package management, [gulp](http://gulpjs.com/) for build scripting, [stylus](http://learnboost.github.io/stylus/) (+ [nib](https://github.com/visionmedia/nib)) as a css preprocessor, and other common development tools such as [jshint](http://www.jshint.com/), [gulp-useref](https://github.com/jonkemp/gulp-useref), [uglifyjs](https://github.com/mishoo/UglifyJS2), [connect](http://www.senchalabs.org/connect/) and [livereload](https://github.com/intesso/connect-livereload). It is meant to be a good starting point but not prescriptive. It is entirely feasible to change any or all parts to fit your development preferences.

The server-side development environment uses Maven and the Jetty webapp container. The server code itself uses [Restlet](http://restlet.com/) and Google [Guice](https://code.google.com/p/google-guice/) which are both heavily used by the Aperture JS server components. A different server architecture can easily be used provided that any Aperture JS services you require are still exposed.


## Setup

Clone this repo locally

```
git clone git@github.com:oculusinfo/aperturejs-bootstrap.git my-aperture-app
```

Ensure that the following development tools are installed:

 1. [JDK (v7+)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
 1. [Apache Maven](http://maven.apache.org/)
 1. [node.js & npm](http://nodejs.org/)

A minimal setup is sufficient for an automated build or CI machine and you'll be able to do everything you need to do, but some commands may be a little clunky. For the best possible development experience, also install:

 1. [bower](http://bower.io/) - `npm install -g bower`
 1. [gulp](http://gulpjs.com/) - `npm install -g gulp`


## Usage

### Development

#### Client

First install all dependencies:
```
npm install
bower install
```
*Note: if you didn't install bower globally you'll need to run `./node_modules/.bin/bower` instead of `bower`

During development the client code is managed and served out by a local node.js express server running on port `9000` (the port is configurable in `gulpfile.js`. The development server will compile (stylus) then livewatch and update the browser on any changes. It proxies all requests to `/aperture` and `/rest` paths to the Java server running on port `8080` which you must run as a seperate process. Any additional paths that need to be opened up to the back end should be added to the proxy configuration settings near the top of `gulpfile.js`.

To run the client-side development build, server, and auto-compile/reload on change use the following command from the `client` directory:
```
gulp serve
```

#### Server

During development the Java servlet only handles "API" requests. html, js, css and other "front-end" files are all handled by the gulp client development server. 

To run the development API server on port `8080` use the following command from the `server` directory:
```
mvn jetty:run
```

Point your favorite browser at `http://localhost:9000` and start developing.


### Production

To build the production WAR file, run the following from the `server` directory:

```
mvn clean install -Denvironment=deployment
```

This will include a stage that does `npm install`, `bower install` and builds the production client distribution. The generated WAR file will contain the entire server and client package. This WAR can be found in `server/target`.

To run and test the production server:

```
mvn jetty:run -Denvironment=deployment
```

Generally the server build process will handle making the production client for a final release. However, the production client build can be executed and tested independently of the back end. To build the front-end, run from the `client` directory:
```
gulp build
```


