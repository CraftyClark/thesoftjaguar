Title: Berlin Buzzwords 2013
Date: 2013-18-11 14:13
Tags: conferences, elasticsearch, solr, lucene, open-source, big-data, java, cassandra, hadoop, pig, search, store, scale
Category: programming
Slug: berlin-buzzwords-2013
Summary: An overview of Berlin Buzzwords main topics, thoughts, comments and more.

Berlin Buzzwords is another of those events that I like to attend, it took place on June 3rd and 4th at ![Kulturbrauerei](http://kulturbrauerei.de/en), which is an awesome place.

Buzzwords is a conference focused in Open Source technologies, mainly related to search engines, big data and NoSQL databases, having three main topics: `search`, `store` and `scale`. The main technologies from the talks that I attended were ![Elasticsearch](http://www.elasticsearch.org/), ![Solr](http://lucene.apache.org/solr/) ![Cassandra](http://cassandra.apache.org/), ![Hadoop](http://hadoop.apache.org/), ![Pig](http://pig.apache.org/), ![Mahout](http://mahout.apache.org/), ![FitNesse](http://fitnesse.org/), ![MongoDB](http://www.mongodb.org/), ![Logstash](http://logstash.net/) and ![Kibana](http://kibana.org/), among others.

Instead of explaining what was going on in each talk that I attended, I have decided to cover the three main areas of the conference.

# Search

This topic covers several search engines, mainly `Elasticsearch`, but also `Solr` (and of course `Lucene` as the base), which are quite famous. But it also wraps how to analyze a lot of log information (or just a lot of data). I mainly attended Elasticsearch's talks, but there were more technologies involved.

## Just search

So we want to get started with a search engine, but... How do we use them? ![Getting down and dirty with Elasticsearch](http://berlinbuzzwords.de/sessions/getting-down-and-dirty-elasticsearch) starts from the basic concepts and explains how to improve our queries (which, IMHO, is the most interesting part of this talk). It differences exact values search (should match **entirely**) from full text search (search within the text), it introduces the *inverted index* concept for improving full text search, which is being done separating words and terms, sorting unique terms and listing docs containing those terms, via the use of **analyzers**. The exact matching is done (or should be done) with **filters**, and the text search is achieved with **queries**, so the filters are faster than the queries and cacheable, but the queries provides the full text search. Finally, the talk addresses some different ways of implementing **autocomplete**: the *N-grams* method, good for partial word matching, the *Edge N-grams* method, perfect for autocomplete (just activating the type `edge_ngram`).

The **analyzers** is a very interesting topic, helping us to deal with different languages at query time. ![Language Support and Linguistics in Lucene/Solr/Elasticsearch and the open source ecosystem](http://berlinbuzzwords.de/sessions/language-support-and-linguistics-lucenesolrelasticsearch-and-open-source-and-commercial-eco) explaines again the tokenization and normalization of the given text query, where the tokens are mapped to the document ids that contain them. However, we will find the [precision and recall](http://en.wikipedia.org/wiki/Precision_and_recall) problem. The talk explains how Lucene, Elasticsearch and Solr deals with this, and how the synonyms can improve the recall. About the latter, the best practice is to apply the synonyms in the query side instead of in the index: it allows synonym updating without reindexing and is easier to turn the synonym feature off.

When we work with NoSQL based solutions, in this case with Elasticsearch, we always have a problem: how to divide the relational data? ![Document relations with Elasticsearch](http://berlinbuzzwords.de/sessions/document-relations-elasticsearch) gives two answers to this problem. The first one is setting the `_parent` field in the desired mapping, for linking the current document (child) to another one (parent). The advantage is that the parent document doesn't need to exist at time of indexing, which improves the performance, and if we want to have the parent documents based on matches with their child ones, we can always set the `has_child` query. The second workaround is the use of **nested objects**. We can set a JSON document with nested fields, instead of defining a parent, using the `nested` field type (which triggers Lucene's *block indexing*). This document will be flattened and the *block indexing* translate the ES document into multiple Lucene documents. With this approach, the root document and its nested documents remain always in the same block, and when querying, you establish it as a nested query, specify the nested level, the score mode and then the query inside that nested document.

## Test driven development in Big Data

The search topic is also closely related to Big Data. What's the point of having tons and tons of data if we can't find anything useful there? Big Data solutions usually are complex and not easy to test. ![Bug bites Elephant?](http://berlinbuzzwords.de/sessions/bug-bites-elephant-test-driven-quality-assurance-big-data-application-development) presents a way of assuring quality data following the Test-driven development process, specifically when using Hadoop, Pig and/or Hive. There are several ways of achieving this (JUnit, MRUnit, iTest or simply using scripts), but the talk introduces FitNesse as a natural language test specification, where the tests are written as stories instead as programming code. The FitNesse server translates the natural language into Java and integrates with REST or Jenkins if needed. This has several advantages, like having a wiki page with the tests written in a language that everyone can understand (or integrate PigLatin directly into that wiki page). However, natural language has its limits, like for instance, if you want to check the expected result (like the output of a Pig job alias).

## Don't drown in a log ocean

Who hasn't had issues when dealing with log information? We want to know what's going on when something is wrong, but sometimes the log is too verbose (Java :P) or simply we have the log files distributed among a lot of servers. ![The State of Open Source Logging](http://berlinbuzzwords.de/sessions/state-open-source-logging) shows some technologies that address this problem, like fluentd, Logstash, Graylog2, ELSA, Flume or Scribe. Therefore, one work around to this problem is to centralize all log files into a Elasticsearch cluster, transforming every log line into a JSON document. The talk is focused on the **Kibana** way, which is a very powerful Logstash and Elasticsearch interface.




![Geospatial Event Detection in the Twitter Stream](http://berlinbuzzwords.de/sessions/geospatial-event-detection-twitter-stream)


# Store


![On Cassandra's Evolution](http://berlinbuzzwords.de/sessions/cassandras-evolutions)
![Cassandra by Example](http://berlinbuzzwords.de/sessions/cassandra-example-data-modeling-cql3)
![JSON logging with Elasticsearch](http://berlinbuzzwords.de/sessions/json-logging-elasticsearch)
![Multi-modal Recommendation Algorithms](http://berlinbuzzwords.de/sessions/multi-modal-recommendation-algorithms)


# Scale

The most relevant talk about `scale`, in my opinion, was ![Elasticsearch in Miniature](http://berlinbuzzwords.de/sessions/scaling-other-way-elasticsearch-miniature), a nice way of presenting Elasticsearch's distributed capabilities. The cool thing is that the Elasticsearch guys installed their system in 5 Raspberry Pi, creating a test ES cluster over WiFi: this allowed them to [show us](http://www.youtube.com/watch?feature=player_embedded&v=AA_gihv5H-Y) things like how the shards and replicas were rebalanced when the number of nodes in the cluster was changing, or what was the cluster state after destroying and creating replicas. The interesting thing is that everything went fine, having into account that this kind of demos usually tend to fail in the final presentation, and the fact that the network wasn't really reliable is a plus.