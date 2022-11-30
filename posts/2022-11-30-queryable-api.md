---
title: "Site update: December, 2022"
summary: A new backend, a new organelles page, and more
thumbnail_url: 'https://raw.githubusercontent.com/janelia-cosem/openorganelle-blog/main/assets/queryable-api_thumbnail.jpg'
carousel_url: 'https://raw.githubusercontent.com/janelia-cosem/openorganelle-blog/main/assets/queryable-api_carousel.jpg'
tags: ["database", "openorganelle"]
authors: ["Davis Bennett"]
date: "2022-11-30"
published: True
---

# Changes

* We now have a news page.

* We are delighted to announce the release of a new backend for OpenOrganelle. This upgrade makes OpenOrganelle more performant and greatly simplifies the process of adding new datasets.

* We have also reworked the [organelles page](https://openorganelle.janelia.org/organelles). 

## A page for news
We added a page for news and updates about the site. This space will be used to announce new datasets, new site features, as well as posts covering the biology revealed in our datasets.  


## Our new database
Historically, the metadata required for displaying a dataset on OpenOrganelle was stored as plain JSON files in a [github repository](https://github.com/janelia-cosem/fibsem-metadata). This repository grew out of my efforts to define JSON-serializable data models in python to represent the different classes of metadata used by Cellmap. 

When a visitor landed on OpenOrganelle, the metadata for each dataset would be fetched from this github repository. For example, here's the metadata for [`jrc_hela-2`](https://github.com/janelia-cosem/fibsem-metadata/blob/master/api/jrc_hela-2/manifest.json). 

This solution was servicable, but it suffered from a fundamental limitation: with each dataset represented as separate JSON document, it was difficult to query relationships across different datasets. If we stored the metadata information as tables in a relational database, then it would be much easier to represent the relational structure in our data. 

For example, we wanted to rework the [organelles page](https://openorganelle.janelia.org/organelles) to show examples of different organelles across all our datasets. With our old backend generating this content required traversing the JSON representation of every single dataset. But with a relational database, the same result could be obtained with a single query, provided we organized our metadata correctly.

Dataset metadata is now stored in a queryable [PostgreSQL](https://www.postgresql.org/) database provided via [Supabase](https://supabase.com/). As a developer, I'm extremely happy with how this has turned out, and I'm excited to grow this database to suit the needs of OpenOrganelle and potentially other tools! If you are interested in querying the OpenOrganelle database, please raise an issue on on our [github page](https://github.com/janelia-cosem/openorganelle) and I can walk you through the process of getting database access. 

## Reworked organelles page
Our new [organelles page](https://openorganelle.janelia.org/organelles) now aggregates observations of organelles and other subcellular structures across all our datasets, and has an improved layout.  
