---
layout: post
type: post
tags: 
  - Nested Docs
  - NodeJS
  - MongoDB
published: true
title: MongoDB Nested docs within arrays
---
## MongoDB lets you store in query form nested documents within arrays, check below example for nested document for ratings and an array field for screenplay authors

    var doc = {
       title: 'Jaws',
       year: 1975,
       director: 'Steven Spielberg',
       rating: 'PG',
       ratings: {
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
      /*note the (.) in the ratings.audience, this is what is used to search through a nested document in the database */
        db.collection('movies').
          find({ 'ratings.audience': {'$gte': 90 }}).
          toArray(function(error, docs) {
   
