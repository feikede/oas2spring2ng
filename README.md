A trip from OAS Spec to Spring Boot server to Angular client - a script

## Techstack

### Open API Spec (aka Swagger)
[The OpenAPI Specification (OAS)](https://swagger.io/specification/) defines a standard, language-agnostic interface to RESTful APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection.

### Swagger Editor
[Swagger Editor](https://swagger.io/swagger-editor/) to design, describe, and document your API on the first open source editor fully dedicated to Swagger-based APIs. [Also on Github](https://github.com/swagger-api/swagger-editor).

### Swagger CodeGen
[Swagger CodeGen](https://swagger.io/swagger-codegen/) to build APIs quicker and improve consumption of your OpenAPI-defined APIs in every popular language with Swagger Codegen. [Also on Github](https://github.com/swagger-api/swagger-codegen).

### Spring Tool Suite™
[We provide a customized all-in-one Eclipse based distribution](https://spring.io/tools) that makes application development easy. The tool suites provide ready-to-use combinations of language support, framework support, and runtime support, and combine them with the existing Java, Web and Java EE tooling from Eclipse. An integrated dashboard in the distribution makes it trivial to install further extensions that add support for technologies such as Pivotal Cloud Foundry, Gradle or Pivotal tcServer.

### Angular
[Angular](https://angular.io/) to build applications and reuse your code and abilities to build apps for any deployment target. For web, mobile web, native mobile and native desktop. Speed & Performance.

## Stubs

### The interface - say who is the king

```yaml
swagger: "2.0"
info:
  description: "Testinterface - tells kings name"
  version: "1.0.1"
  title: "SayHello"
basePath: "/v1"
tags:
- name: "questions"
- name: "answers"
schemes:
- "http"
paths:
  /whosthere:
    get:
      tags:
      - "questions"
      summary: "Ask whos there"
      description: "What do you think"
      operationId: "whoIsThere"
      consumes:
      - "application/json"
      produces:
      - "application/json"
      parameters:
      - in: "query"
        name: "name"
        description: "Tell me your name"
        required: true
        type: string
      responses:
        200:
          description: "successful operation"
          schema:
            $ref: "#/definitions/Anyone"
        405:
          description: "Invalid input"
definitions:
  Anyone:
    type: "object"
    properties:
      kingsName:
        type: "string"
```

### SwaggerGen settings for Spring Server

### SwaggerGen settings for angular client

### Spring Boot starter for REST Servers

### Angular starter for simple SPA

## Wiring all together
