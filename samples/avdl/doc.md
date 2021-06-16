# AVDL

## Introduction

This document defines Avro IDL, a higher-level language for authoring Avro schemata. Before reading this document, you should have familiarity with the concepts of schemata and protocols, as well as the various primitive and complex types available in Avro.

## Overview

Purpose
The aim of the Avro IDL language is to enable developers to author schemata in a way that feels more similar to common programming languages like Java, C++, or Python. Additionally, the Avro IDL language may feel more familiar for those users who have previously used the interface description languages (IDLs) in other frameworks like Thrift, Protocol Buffers, or CORBA.

## Usage

Each Avro IDL file defines a single Avro Protocol, and thus generates as its output a JSON-format Avro Protocol file with extension .avpr.

To convert a .avdl file into a .avpr file, it may be processed by the idl tool. For example:

```
$ java -jar avro-tools.jar idl src/test/idl/input/namespaces.avdl /tmp/namespaces.avpr
$ head /tmp/namespaces.avpr
{
  "protocol" : "TestNamespace",
  "namespace" : "avro.test.protocol",
```

The idl tool can also process input to and from stdin and stdout. See idl --help for full usage information.

A Maven plugin is also provided to compile .avdl files. To use it, add something like the following to your pom.xml:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.avro</groupId>
      <artifactId>avro-maven-plugin</artifactId>
      <executions>
        <execution>
          <goals>
            <goal>idl-protocol</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

## Defining a Protocol in Avro IDL

An Avro IDL file consists of exactly one protocol definition. The minimal protocol is defined by the following code:

```avro
protocol MyProtocol {
}
```

This is equivalent to (and generates) the following JSON protocol definition:

```jsonc
{
    "protocol": "MyProtocol",
    "types": [],
    "messages": {}
}
```

The namespace of the protocol may be changed using the @namespace annotation:

```avro
@namespace("mynamespace")
protocol MyProtocol {
}
```

This notation is used throughout Avro IDL as a way of specifying properties for the annotated element, as will be described later in this document.

Protocols in Avro IDL can contain the following items:

-   Imports of external protocol and schema files.
-   Definitions of named schemata, including records, errors, enums, and fixeds.
-   Definitions of RPC messages

## Imports

Files may be imported in one of three formats:

-   An IDL file may be imported with a statement like:
    ```avro
    import idl "foo.avdl";
    ```
-   A JSON protocol file may be imported with a statement like:
    ```avro
    import protocol "foo.avpr";
    ```
-   A JSON schema file may be imported with a statement like:
    ```avro
    import schema "foo.avsc";
    ```
    Messages and types in the imported file are added to this file's protocol.

Imported file names are resolved relative to the current IDL file.

<!---
cspell:words CORBA avpr avdl avsc
 --->
