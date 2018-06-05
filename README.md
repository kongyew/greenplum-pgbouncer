# Readme
This repository provides examples on how to scale Greenplum with PGBouncer.

[![Build Status](https://travis-ci.org/kongyew/greenplum-gphdfs-examples.svg?branch=master)](https://travis-ci.org/kongyew/greenplum-gphdfs-examples)

# Introduction
Whenever you want to scale Greenplum cluster, you will need to use connection pool to scale the number of connections that are accessing Greenplum cluster. By default, Greenplum supports a maximum of 500 (TBD) connections.


# Caveat:
PgBouncer does not yet support SSL connections


# Use Case:
1. [Learn how to configure and setup PgBouncer](#usecase1/README.MD)


# Reference:
[Greenplum](https://greenplum.org)
[Pivotal Greenplum](https://pivotal.io/pivotal-greenplum)
[Pgbouncer](https://pgbouncer.github.io/)
[Docker with Pgbouncer](https://github.com/brainsam/pgbouncer)
[Scaling Greenplum with PgBouncer](https://www.linkedin.com/pulse/scaling-greenplum-pgbouncer-sandeep-katta-/)
