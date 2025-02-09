[![Build Status](https://api.travis-ci.org/AODocs/endpoints-framework-maven-plugin.svg?branch=master)](https://travis-ci.org/AODocs/endpoints-framework-maven-plugin)
# Endpoints Framework Maven plugin

This Maven plugin provides goals and configurations to build Endpoints Framework projects.

# Requirements

Maven is required to build the plugin. To download Maven, follow the [instructions](http://maven.apache.org/).

The remaining dependencies are specified in the pom.xml file and should be automatically downloaded when the plugin is built.

# How to use

In your Maven App Engine Java app, add the following plugin to your pom.xml:

```XML
<plugin>
  <groupId>com.aodocs.endpoints</groupId>
  <artifactId>endpoints-framework-maven-plugin</artifactId>
  <version>2.5.3</version>
</plugin>
```
All goals are prefixed with `endpoints-framework`

## Server

The plugin exposes the following server side goals
* `clientLibs` - generate client libraries
* `clientSrc` - generate client code locally. Requires `python 2.7` and the python package `google-apis-client-generator`
* `discoveryDocs` - generate discovery docs
* `openApiDocs` - generate Open API docs

The plugin exposes the following parameters for configuring server side goals
* `discoveryDocDir` - The output directory of discovery documents
* `clientLibDir` - The output directory of client libraries
* `generatedSrcDir` - The output directory of generated endpoints source
* `openApiDocDir` - The output directory of Open API documents
* `serviceClasses` - List of service classes (optional), this can be inferred from web.xml
* `webappDir` - Location of webapp directory
* `hostname` - To set the hostname of the root url for Open API docs, discovery docs, and client libs (ex: `hostname = myapp.appspot.com` will result in a default root url of `https://myapp.appspot.com/_ah/api`)
* `basePath` - To set the base path of the root url for Open API docs, discovery docs and client libs (ex: `basePath = /_ah/api` will result in a default root url of `https://myapp.appspot.com/_ah/api`)

#### Usage
Make sure your web.xml is [configured to expose your endpoints](https://cloud.google.com/endpoints/docs/frameworks/java/required_files) correctly.

No configuration parameters are required to run with default values
```shell
mvn compile endpoints-framework:clientLibs
mvn compile endpoints-framework:discoveryDocs
```

## Client

The plugin exposes the following client side goals
* `generateSrc`

The plugin exposes the following parameters for client side goals
* `generatedSrcDir` - The output directory of generated endpoints source
* `discoveryDocs` - List of discovery docs to generate source from

#### Usage
Client consuming endpoints using the client plugin need to configure the location
of source discovery documents and for best results configure the generateSrc task
in the default generate sources phase.

```XML
<plugin>
  <groupId>com.google.cloud.tools</groupId>
  <artifactId>endpoints-framework-maven-plugin</artifactId>
  ...
  <configuration>
    <discoveryDocs>
      <discoveryDoc>src/endpoints/myApi-v1-rest.discovery</discoveryDoc>
    </discoveryDocs>
  </configuration>

  <executions>
    <execution>
      <phase>generate-sources</phase>
      <goals>
        <goal>generateSrc</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

Users will also need to include the google api client library for their generated
source to compile

```XML
<dependency>
  <groupId>com.google.api-client</groupId>
  <artifactId>google-api-client</artifactId>
  <version>xx.yy.zz</version>
</dependency>
```

Running compile should automatically include generated sources from discovery documents
```shell
mvn compile
```
