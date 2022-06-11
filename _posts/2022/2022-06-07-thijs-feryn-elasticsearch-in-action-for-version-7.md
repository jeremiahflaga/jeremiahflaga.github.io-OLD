---
layout: post
title: 'Thijs Feryn''s Elasticsearch in Action exercises for version 7'
subtitle: 
categories: [Programming]
tags: [Programming]
date: 2022-06-07 12:00:00 AM UTC
published: false
---

<!-- June 1, 2022 11:20 PM  Philippine Time - started -->



["ElasticSearch in action"](https://www.youtube.com/watch?v=oPObRc8tHgQ) by Thijs Feryn is a very good introduction to Elasticsearch. But its accompanying exercises, which are found [here](https://github.com/ThijsFeryn/elasticsearch_tutorial), are written for version 5 of Elasticsearch.


<!-- from https://codegena.com/change-youtube-thumbnail-in-embed-player/ -->
<!--Embed Code Start-->
<div class="youtube" id="oPObRc8tHgQ" src="https://image.slidesharecdn.com/elasticsearchcodemotion16-160329141159/85/elasticsearch-in-action-1-320.jpg" style="width:560px; height:315px;"></div>
<script type="text/javascript" src="https://codegena.com/assets/js/youtube-embed.js"></script>
<!--Embed Code End-->

If you execute the exercises using ES 7, you will get errors that look like this<sup id="footnote-indicator-types-removed">[[1]](#footnote-types-removed)</sup>:

{: .text-danger .small }
> #! Deprecation: [types removal] Specifying types in document get requests is deprecated, use the /{index}/_doc/{id} endpoint instead.

So I have rewritten some of the queries in those exercises so that they will execute without errors in version 7 of Elasticsearch. Please go to the [original page](https://github.com/ThijsFeryn/elasticsearch_tutorial) to read the description of each exercise, then return here to load the exercises.

[Load exercise 1]()




-----

**Footnotes:**

<div class="small" markdown="1">
<sup id="footnote-types-removed">[[1]](#footnote-indicator-types-removed)</sup> That is because starting version 6, these what they call "Maping Types" was deprecated, and was completely removed in version 7. [A blog post](https://www.elastic.co/blog/removal-of-mapping-types-elasticsearch) in Elasticsearch's website says this:

> - Indices created in Elasticsearch 6.0.0 or later may contain a single mapping type only.
> - Indices created in 5.x with multiple mapping types will continue to function as before in Elasticsearch 6.x.
> - Mapping types will be completely removed in Elasticsearch 7.0.0 although some backwards compatibility features will only be removed in 9.0.0.

That blog post also contains the explanation on [why Mapping Types was removed](https://www.elastic.co/blog/removal-of-mapping-types-elasticsearch#why).

So before ES version 6, a comparison of RDBMS and Elasticsearch looks like this:

{: .table .table-striped .table-sm .col-md-4 .col-sm-6}
|   RDBMS   |   ES      |
| --------- | --------- |
|  Database |  Index    |
|  Table    |  Type     |
|  Row      |  Document |

But for version 6, 7, & 8, it looks like this (refer to [this](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/_mapping_concepts_across_sql_and_elasticsearch.html)):

{: .table .table-striped .table-sm .col-md-4 .col-sm-6}
|  RDBMS   |   ES      |
| -------- | --------- |
|  Table   |  Index    |
|  Row     |  Document |
|  Column  |  Field    |

</div>






From <https://www.youtube.com/watch?v=oPObRc8tHgQ&ab_channel=Codemotion> 




Starting version 6

https://dattell.com/wp-content/uploads/2020/10/Elasticserach-Index_table-500x323.png


https://github.com/ThijsFeryn/elasticsearch_tutorial





Removal of Mapping Types in Elasticsearch 6.0
https://www.elastic.co/blog/removal-of-mapping-types-elasticsearch

