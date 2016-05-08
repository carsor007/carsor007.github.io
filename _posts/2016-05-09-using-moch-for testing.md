---
published: false
---
 Mocha uses behaviour driven development(BDD). It uses the describe and it functions in place of J unit style suites and tests. BDD is designed to read more like stories than tests. Mocha does not come with it's own assertion framework, this means you can use whichever you choose. Node.js  has a built in assert module that is good enough, that's what we used in the example below. 
 To run the test, you have to use Mocha's executable path, this is installed in > $YOUR_PROJECT/node_modules/mocha/bin/mocha. You use this command:> ./node_modules/.bin/mocha test.js. You can add it in your executable


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