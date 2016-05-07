---
published: false
---
## MongoDB lets you store in query form nested documents within arrays, check below example for nested document for ratings and an array field for screenplay authors

  ~~~ bash
  var doc = {
   title: 'Jaws',
   year: 1975,
   director: 'Steven Spielberg',
   rating: 'PG',
   rating: {
      critics: 80,
      audience: 97
    },
    screenplay: ['Peter Benchley', 'Carl Gotlieb']
  };

   db.collection('movies').insert(doc, function(error, result) {
    if (error) {
      console.log(error);
      process.exit(1);
    }

     db.collection('movies').
      find({ screenplay: 'Peter Benchley' }).
      toArray(function(error, docs) {
   ~~~
