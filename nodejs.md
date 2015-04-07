# NodeJS best practices

## Package management
Every package should be referenced from the [npm main repo](https://www.npmjs.org) or the private Worldline npm repo.    
The choice of a package should be done with wisdom, functionnalities, usage and stability are among the criteria that should be evaluated.   
The version reference should use the version number referenced with tilde. References using star or carret are prohibited as they could brake the current usage. 

## Build management
Build should be controlled and managed using [Grunt.JS](http://gruntjs.com/) or [Gulp.JS](http://gulpjs.com/).    
The code syntax should be ensured using the [jshint](http://jshint.com/) plugin (a template is provided in this repo).    
Code coverage should be ensured using [istanbul](https://github.com/gotwarlost/istanbul)

## Testing
Recommended framework for unit testing is [mocha](https://github.com/mochajs/mocha). A unit test should only have one assertion. Developpers have to keep the test as simple as possible to keep up maitenability.  
[Chai](http://chaijs.com/) can be used to make litteral assertion.
Functionnal testing could be achieved from the same runtime using [supertest](https://github.com/tj/supertest).    
Code coverage should be **above 80%** for a REST backend application. 

### Bencharmking
You can use any conventionnal tool to bench your node.JS application such as [Gatling](http://gatling.io/) or [Jmeter](http://jmeter.apache.org/).   
To bench a project that uses web sockets a more specific solution should be required such as [websocket-bench](https://github.com/M6Web/websocket-bench)   

## REST API implementation

We recommend to use [Express](http://expressjs.com/) to implement REST apis. Express is a standard used in most of the nodeJS applications.   
Code has to be splitted into routers and sub routers.     

[hapi](http://hapijs.com/) could be an alternative.

## Code quality

### Callback hell
Writting nodeJS code requires a lot of the nested callback which will end up with a code too much horizontal.     
We recommend to use the [async](https://github.com/caolan/async) node module to avoid this problem.   
Async.waterfall should always be used when there is more than 2 callbacks nested.    
Otherwise you can use promise (but not both in the same project to keep consistency)

### Project organization
The project should be organized using the following folders:

Mandatory:
* routes: the entry point for HTTP requests
* test: test files

Optionnal:
* clients:  web service clients
* services: advanced business implementation
* tools: the common code such as logging or error handling
* web: the web interface

### Don't Repeat Yourself
In order to achieve the DRY principle developpers should use the middleware feature of Express.JS framework.  

## Fault tolerance
Every backend should support failure from its dependencies (databases, web services...).    
End users always have to be served in time whether with the request content or with an error.   
We reccommend to use the [toobusy](https://github.com/lloyd/node-toobusy)  node module to prevent server overloading. The toobusy plugin monitors the event loop and lets us react to request stacking up.    

```javascript
if (toobusy()) {
    res.send(503, "I'm busy right now, sorry.");
  } else {
    next();
}
```
     
For application that highly uses I/O, the cicuit breaker template could be implemented in order to ensure gracefull service degradation.

## Logging
For **production environment** logging should be reduced to the strict minimum:
* full incoming and outgoing request (without paylod for POST and PUT requests)
* full error information
* other key informations to recreate the behavior in another environement

For **stagging** environments detailed informations about what's happening while handling the request should be logged.   
Someone who doesn't know anything about the application should be able to understand what's happening and to debug. 
The debug log level shouldn't log more than 20 lines per requests. If you need to write more logs consider using a lower log level like trace.    
 
**Every log line should have a timestamp and a node indentifier. We need to be able to track the logs for any single request.**

## Configuration
We recommend to use the [nconf](https://github.com/flatiron/nconf) node module for configuration management.
## Documentation
In addition to the project documentation every project should include a readme.md file intend for the developper which describes how to get the application up and running and the project specifics if any.
