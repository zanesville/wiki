---
title: Sanitary Lines Cleaning App
tags: nodejs
---
This application is part of the postgres-forms-v2 set of applications that read and write directly from the Postgres database. These are built in NodeJS, use Fastify as the server and node-postgres for reading and writing to Postgres.

## Key Features
 - joins the ``utl_sanitary_lines_cleaning table`` to the ``utl_sanitary_lines layer`` via the ``uuid`` field
 - the ``uuid`` field in the ``utl_sanitary_lines`` layer is auto-created on line create in QGIS **NOT IN POSTGRES**
