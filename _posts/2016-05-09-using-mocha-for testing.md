---
layout: post
type: post
tags: 
  - Testing
  - NodeJS
  - Mocha
published: true
title: Using Mocha for tesing
---
 Mocha uses behaviour driven development(BDD). It uses the describe and it functions in place of J unit style suites and tests. BDD is designed to read more like stories than tests. Mocha does not come with it's own assertion framework, this means you can use whichever you choose. Node.js  has a built in assert module that is good enough, that's what we used in the example below. 
 To run the test, you have to use Mocha's executable path, this is installed in     $YOUR_PROJECT/node_modules/mocha/bin/mocha. You use this command:    ./node_modules/.bin/mocha test.js. You can add it in your executable


    ## file test.js below

    var assert =  require('assert');

    describe('my feature', function() {
        it('works', function() {
         assert.equal('A', 'A');
        });
        it('fails gracefully', function() {
         assert.throws(function() {
          throw 'Error!';
         });
        });
     });

    describe('my other feature', function() {
        it('async', function(done) {
         setTimeout(function() {
          done();
         }, 25);
        });
     });

    ## Below should be the output.

    $./node_modules/.bin/mocha test.js

      my feature
         works
         fails gracefully
  
      my other feature
         async

      3 passing (38ms)
    $

You can also use Gulp to automate the Mocha tests. When you run gulp, it waits for a file to change, when it changes it runs your tests and then it reports the results and goes back to waiting for more file changes. This enables you to get fast feedback on your work since you dont have to re-type your test command to run your tests. All you have to do is hit Control-S.

Include it in your package.json like below:

    {
    "dependencies": {
     "gulp": "3.8.11",
     "gulp-mocha": "2.0.1"
     "mocha": "2.2.4"
      }
    }

Gulp is separated by tasks listed in a file gulpfile.js.

    Below is a gulp file example with 2 tasks, test and watch.

    var gulp = require('gulp');
    var mocha = require('gulp-mocha');

    gulp.task('test', function() {
        gulp.
          src('./test.js').
          pipe(mocha()).
          on('error', function(err) {
            this.emit('end');
          });
     });
      /* the watch task below watches all the .js files in the cwd and re-runs the test task when any of the files change*/
    gulp.task('watch', function() {
        gulp.watch('./*.js', ['test']);
    });

