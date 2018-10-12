---
description: 'Array, Linked List, Stack, Queen, Hashes, Tree'
---

# data structure review in java

## Array and ArrayList

## Linked List

## Stack

## Queen

## Hashes

### description

a data structure which organize data using hash functions in order to support quick insertion and search.

There are two different kinds of hash tables: hash set and hash map.

the hashset is a set, which has no repeat value in set

the hashmap is  map which insert key can find the responding value. Actually, it will translate key into a hashcode and then find the value to key.

### what is the principle of hash table

In order to support quick insertion and search, the hash function the key principle.

#### hash function

By using hash function, the search space of key can be narrow, thus, the responding value can be found quickly.

One of the hash function is module function: hashcode % table size= hash \(we could imagine the hash  is a numbers of the buckets \). Basically, both hashcode1 and hashcode2 could in the same bucket if they get same number when module by a int number.

Thus, if hashcode is 1-50, and table size is 50, then every hashcode will be store in 0-49 buckets respectively.

#### collision

If there are hashes in the same bucket,  that is collision.

How to deal with collision

* 
### how to design a hash

### function

### hash in java

### important question

## Tree

