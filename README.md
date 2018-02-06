A trip from OAS Spec to Spring Boot server to Angular client - a script I used for giving a hands-on lesson to programmers.


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

### The interface - say hello

```yaml
swagger: "2.0"
info:
  description: "Testinterface"
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

### SwaggerGen settings for our Spring Server

```json
{
"basePackage":"com.example.api",
"configPackage":"com.example.api.spring.config",
"apiPackage":"com.example.api.spring.server",
"modelPackage":"com.example.api.spring.model",
"groupId":"com.example.api",
"artifactId":"springserver-v1",
"developerName":"Rainer Feike",
"developerOrganization":"Superpower Corp.",
"licenseName":"Apache 2.0",
"java8":true,
"title":"hello springserver",
"interfaceOnly":"true"
}
```

Or ```ìnterfaceOnly:false``` if you want codegen to generate a complete server.

### SwaggerGen

Useful alias: ```codegen223='java -jar /Users/rainer/bin/swagger-codegen-cli-2.2.3.jar '```

Show help for a language: ```codegen223 config-help -l spring```

Generate code: ```codegen223 generate -i test.yml -o target -l spring -c spring_options.json```

Then go to ```cd target```and ```mvn install``` that in your local m2 repo.




### Spring Boot Web starter

Create your Spring Boot app stub.
```
http://start.spring.io/starter.zip?name=demo-1&groupId=de.beispiel&artifactId=demo-1&version=0.0.1-SNAPSHOT&description=testtest&packageName=de.beispiel.server2&type=maven-project&packaging=jar&javaVersion=1.8&language=java&bootVersion=1.5.9.RELEASE&dependencies=web
```

Add our java interface to dependencies.
```
    <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>com.example.api</groupId>
			<artifactId>springserver-v1</artifactId>
			<version>1.0.1</version>
		</dependency>
```

Then maybe update your maven dependencies and
don't forget a root context in application.properties:

```
server.contextPath=/v1
server.port=8080
```

Implement the controller endpoint class, ie like so:

```
@Controller
@CrossOrigin
public class WhosController implements WhosthereApi {

	@Override
	public ResponseEntity<Anyone> whoIsThere(String name) {
		return new ResponseEntity<>(new Anyone().kingsName(name), HttpStatus.OK);
	}
}
```

### SwaggerGen settings for angular client
Nothing special, just type
```codegen223 generate -i test.yml -o tsclient -l typescript-angular2```

### Angular starter for simple SPA
Create a simple angular app ([like here](https://angular.io/guide/quickstart)),
and add a service:

```bash
ng new testapp
cd testapp
ng generate service testservice
```

Add the generated typescript-angular2 client to the new app (we copy it here, using a NPM repo would be better):

```bash
cp -a tsclient testapp/src/app
```

In your testservice, add a wrapper for the rest call like so:

```typescript
@Injectable()
export class TestserviceService {

  constructor(private client : QuestionsApi) {}

  request(name : string): Promise<Anyone> {
    return this.client.whoIsThere(name).toPromise();
  }

}
```

Call the backend in your app.component.ts

```typescript
export class AppComponent {
  title = 'app';
  feedback = '';

  constructor(private service : TestserviceService) {}

  onClick() {
    this.service.request("weristda").then((rc) => this.feedback = rc.kingsName)
      .catch( (err) => this.feedback = 'Service unavailible' );
  }
}
```

And wire that in your html, i.e. with a button like so:

```html
<button (click)="onClick()">Press this button</button>
<h1>{{feedback}}</h1>
```

Add the missing providers to your app.module.ts

```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpModule
  ],
  providers: [TestserviceService, QuestionsApi],
  bootstrap: [AppComponent]
})
```

## Wiring all together
Nothing special to to, it just works (maybe change the basepath in QuestionsApi to http://localhost:8080/v1) :-)

