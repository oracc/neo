# neo -- New Easier Oracc
## Introduction

Neo is a project which aggregates other project glossaries and packages them in Oracc's JSON format for use with the new Oracc search.  There are two parts to Neo: the glossaries proper that are ingested into the ElasticSearch setup, and the glossary articles which must be installed on the server where ElasticSearch is running.

## Definitions

In this documentation we write BUILD_HOST for the host on which Neo is built, e.g., https://build-oracc.museum.upenn.edu; we write SEARCH_HOST for the host where ElasticSearch is running.  These two hosts can be the same or different.

## Overview 

Neo does not contain any data; it harvests its data from the projects listed in 00lib/order.lst.  Oracc is configured so that Neo can be built with a standard ```oracc build``` command.

After Neo has been built, the JSON glossaries are publically available for download at BUILD_HOST/neo/downloads so they can be retrieved locally or remotely.  

The glossary files must also be installed on SEARCH_HOST.  If BUILD_HOST and SEARCH_HOST are the same no further action is needed.  If SEARCH_HOST is a separate machine the command ```oracc serve``` will set the new build up so that it can be picked up by all Oracc hosts that are configured to check BUILD_HOST for new served packages.

## Glossary JSON

The glossary JSON that should be used by ElasticSearch is in BUILD_HOST/neo/downloads; in the file system on BUILD_HOST they are in /home/oracc/www/neo/downloads.  In addition to the compressed glossaries themselves there is a manifest file, **GLOSSARIES**, which lists the glossary files to support iterating over the glossaries to retrieve them all.

The **GLOSSARIES** file is the last file written by Neo's ```oracc build``` process so it is also a timestamp for the current glossary build.

## Glossary Articles

The glossary articles are created on BUILD_HOST before the glossary JSON is built; they are served to SEARCH_HOST after the glossary JSON is built.  This means that when building indexes in a context where BUILD_HOST=SEARCH_HOST no synchronization checks are necessary. 

When building on a separate SEARCH_HOST, however, it is advisable to check that the installed glossary articles are later than the glossary JSON and this can be done by comparing the timestamp on the **GLOSSARIES** manifest with the SEARCH_HOST file **/home/oracc/neo/installstamp**.  The **installstamp** file is touched by the serve-installation process at the end of installation and indexing; it does not exist on BUILD_HOST, only on SEARCH_HOST, after a served package has been installed and indexed.

Note that there can be a delay between the appearance of a new **GLOSSARIES** manifest and the updating of **installstamp**.  Oracc servers by default check for new packages every 10 minutes.  All the new packages are retrieved, then they are all unpacked/installed, then they are all indexed.  When a very large set of packages (like ePSD2) is updated this can mean that there is an hour or more between running ```oracc serve``` on the BUILD_HOST and the final indexing of the served package on SEARCH_HOST.



