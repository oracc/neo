# neo -- New Easier Oracc
## Introduction

Neo is a project which aggregates other project glossaries and packages them in Oracc's JSON format for use with the new Oracc search.  There are two parts to Neo: the glossaries that are ingested into the ElasticSearch setup, and the glossary files which must be installed on the server where ElasticSearch is running.

## Definitions

In this documentation we write BUILD_HOST for the host on which Neo is built, e.g., https://build-oracc.museum.upenn.edu; we write SEARCH_HOST for the host where ElasticSearch is running.  These two hosts can be the same or different.

## Overview 

Neo does not contain any data; it harvests its data from the projects listed in 00lib/order.lst.  Oracc is configured so that Neo can be built with a standard ```oracc build``` command.

After Neo has been built, the JSON glossaries are publically available for download at BUILD_HOST/neo/downloads so they can be retrieved locally or remotely.  The glossary files must also be installed on SEARCH_HOST.  If BUILD_HOST and SEARCH_HOST are the same no further action is needed.  If SEARCH_HOST is a separate machine the command ```oracc serve``` will set the new build up so that it can be picked up by all Oracc hosts that are configured to check BUILD_HOST for new served packages.

