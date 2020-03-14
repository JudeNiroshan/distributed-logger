# Distributed Logger

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Abstract](#abstract)
  - [Requirements](#requirements)
- [Architecture](#architecture)
- [Overview](#overview)
  - [Execution workflow](#execution-workflow)
- [How to run](#how-to-run)
- [Future improvements](#future-improvements)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Abstract
This is the documentation project which explains the system design and architecture of 
a distributed logger solution. This entire setup is a proto-type implementation. Basic idea 
was to experiment the communication between 2 different applications using Google Protocol 
Buffers.

### Requirements
>The first application receives logs of events via HTTP and pass them to the second application 
for storing them in a file.  

>The API have 1 route where a user may send events (JSON objects with data) via POST calls.

>Once an event is received it must be serialized in Google Protocol Buffers format and passed 
>on to the second application for persistence.

## Architecture
There are 3 different projects needs to be integrated with each-other to make the solution work.

1) Audit-Server _(REST API which receives events via HTTP)_
2) logger-eureka-server _(server used for service discovery)_
3) logger-app _(handle event data)_

## Overview

![overview](resources/overview.jpg?raw=true "Overview")

### Execution workflow
1) logger-app instance will register to logger-eureka-server as a client
2) User send POST request to audit-server
3) audit-server request logger-app URIs from eureka server
4) audit-server receives available logger-app URIs
5) audit-server create a connection channel to logger-app URI
6) logger-app send the response
7) audit-server send the HTTP response back to the user

## How to run

- Setup [logger-eureka-server](https://github.com/JudeNiroshan/logger-eureka-server) and start up the server
- Setup [logger-app NodeJS](https://github.com/JudeNiroshan/logger-app) application and start it
- Setup [audit-server](https://github.com/JudeNiroshan/audit-server) and start the application
- Use Postman application or make a curl request to audit-server endpoint

Sample HTTP request JSON payload:
```
    {
	"timestamp": 15623276532,
	"userId": 1029,
	"event": "something happened. Please log this in a file"
    }
```

## Future improvements

Because of the time constraint, following implementations were not completed.  

- Use a separate project to maintain the `.protoc` files. 
- Use a spring cloud configuration project to manage the configurations.
- Write unit and integration tests in audit-server application.

