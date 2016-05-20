---
layout: post
title:  Chewy! - Elasticsearch
date:   2016-05-15
categories: #ruby #elasticsearch
---

>> Multi-model indices.

>> Index classes are independent from ORM/ODM models. Now, implementing e.g. cross-model autocomplete is much easier. You can just define the index and work with it in an object-oriented style. You can define several types for index - one per indexed model.

>> Every index is observable by all the related models.

>> Most of the indexed models are related to other and sometimes it is necessary to denormalize this related data and put at the same object. For example, you need to index an array of tags together with an article. Chewy allows you to specify an updateable index for every model separately - so corresponding articles will be reindexed on any tag update.

>> Bulk import everywhere.

>> Chewy utilizes the bulk ES API for full reindexing or index updates. It also uses atomic updates. All the changed objects are collected inside the atomic block and the index is updated once at the end with all the collected objects. See Chewy.strategy(:atomic) for more details.

>> Powerful querying DSL.

>> Chewy has an ActiveRecord-style query DSL. It is chainable, mergeable and lazy, so you can produce queries in the most efficient way. It also has object-oriented query and filter builders.

>> Support for ActiveRecord, Mongoid and Sequel.

https://github.com/toptal/chewy
