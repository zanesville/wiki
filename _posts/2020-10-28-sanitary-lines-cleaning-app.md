---
title: Sanitary Lines Cleaning App
tags: nodejs
category: apps

---
This application is part of the postgres-forms-v2 set of applications that read and write directly from the Postgres database. These are built in NodeJS, use Fastify as the server and node-postgres for reading and writing to Postgres.

## Key Features
 - The app relies on a join between the ``utl_sanitary_lines_cleaning table`` to the ``utl_sanitary_lines layer`` via the ``uuid`` field
 - The ``uuid`` field in the ``utl_sanitary_lines`` layer is auto-created on save in QGIS **NOT IN POSTGRES**.
 - I am also adding the pipe segment id as a backup method to use down the road
 - This app is so we can keep track of what lines have been cleaned for reporting purposes - i.e. we have cleaned 20% of the lines in the last X years
