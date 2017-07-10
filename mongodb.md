### MongoDB

0. Terms

SQL | MongoDB
-- | --
Table | Collection
Row   | Document
Column | Field
primaryKey | primaryKey

1. Create Collection
```ts
testCollection = new Mongo.Collection('...');
```
2. Insert Data
```ts
testCollection.insert({
  xx: xx,
  xx: xx
});
```

3. Find Data
 - get all the data
```ts
testCollection.find();
```
```ts
// retrieves data from collection as an array of objects
testCollection.find().fetch(); //human-readable format
```
- find specific data
```ts
testCollection.find({
  xx: xx
}).fetch();
```

 - count documents returned by `find()`
 ```ts
testCollection.find({
  xxx: xxx
}).count();
 ```

 - `findOne` stop searching as soon as a single match is found
 ```ts
 testCollection.findOne({
   xxx: xx
 })
 ```


 4. Update Data

By default, the update function works by deleting the original document that’s being updated and then creating an entirely new document with the data that we specify.

```ts
  // The first argument selects the document,
  //the second argument updates field
  testCollection.update({
    _id: xxx
  }, {
    xx: xx
  })
```

In order to avoid that, we can take advantage of `$set` operator as the second argument, which allows us to midify the value of a specific field/multiple fields without deleting the original document
```ts
testCollection.update({
  _id: xxx
}, {
  $set: {
    xx: xx
  }
});
```

In order to increment a value, we could use `$inc` operator
```
testCollection.update({
  _id: xxx
}, {
  $inc: {
    xx: 6 //increment by 6
  }
})
```

> Update an element by index
```
// use `` for an expression, ${xx} for passing a variable
testCollection.update(id, { $set: { [`xx.${index}.xx`]: value } });
```

5. Sort Document

With `find()` method, we can retrieve data from the collection. Although `collection.find()` and `collection.find({})` are basically doing the same thing, but with the second one, we can pass a second argument for filtering data
```ts
// sort the documents by value of score field in descending order
// if 2 scores are equal, sort them by name in alphabetical order
return testCollection.find({}, { sort: {score: -1, name: 1} });
```

6. Remove data

```
testCollection.remove({
  xx: xx
});
```

7. Access database from server side
```
1. Meteor mongo
2. show collection
3. db.[collectionName].xxxus
```