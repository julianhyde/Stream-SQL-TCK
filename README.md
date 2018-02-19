<!--
{% comment %}
Licensed to the Apache Software Foundation (ASF) under one or more
contributor license agreements.  See the NOTICE file distributed with
this work for additional information regarding copyright ownership.
The ASF licenses this file to you under the Apache License, Version 2.0
(the "License"); you may not use this file except in compliance with
the License.  You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
{% endcomment %}
-->
[![Build Status](https://travis-ci.org/Stream-SQL-TCK/Stream-SQL-TCK.svg?branch=master)](https://travis-ci.org/julianhyde/Stream-SQL-TCK)

# Stream SQL TCK

This project is a test compatibility kit (TCK) for streaming SQL.

If you are the author of an engine that supports streaming extensions
to SQL, you can run these tests to find out whether your engine is
compliant.

This project is more than a test suite, it is a living
specification. The tests in the latest release represent what is
considered valid; new features are under development in branches, pull
requests and issues, and when complete, will be voted upon in the next
release. Many of the members participate in projects of the Apache
Software Foundation, and we run this project according to the Apache
Way.

## Scripts

Each tests is called a *script* and consists of:

* One or more stream definitions
* A query (or possibly several) based on those streams
* Records to insert into the stream(s) at particular times
* Expected observations, such as which records should be emitted from
  the query at particular times

A script is defined using a Java API. Here is a simple example:

```java
  public Script selectWhere() {
    return Script.builder()
        .definitions()
        .stream("Orders")
        .timestamp("rowtime").notNull()
        .integer("orderId").notNull()
        .varchar("product", 20).notNull()
        .end() // stream "Orders"
        .end() // definitions
        .query("Q", "select orderId from Orders where product = 'milk'")
        .input()
        .insert("ORDERS", 0, 100, "beer")
        .insert("ORDERS", 1, 101, "milk")
        .end() // input
        .expect()
        .row("Q", 101)
        .end() // expect
        .build();
  }
```

There are more examples in [Basic.java]
(src/main/java/net/hydromatic/streamsqltck/basic/Basic.java).

## Get Stream-SQL-TCK

### From Maven

Get Stream-SQL-TCK from
<a href="https://search.maven.org/#search%7Cga%7C1%7Ca%3Astream-sql-tck">Maven central</a>:

```xml
<dependency>
  <groupId>net.hydromatic</groupId>
  <artifactId>stream-sql-tck</artifactId>
  <version>0.1</version>
</dependency>
```

### Download and build

You need Java (1.8 or higher; 1.9 preferred), git and maven (3.5.2 or higher).

```bash
$ git clone git://github.com/Stream-SQL-TCK/Stream-SQL-TCK.git stream-sql-tck
$ cd stream-sql-tck
$ mvn compile
```

## More information

* License: <a href="LICENSE">Apache Software License, Version 2.0</a>
* Author: Julian Hyde
* Source code: http://github.com/Stream-SQL-TCK/Stream-SQL-TCK
* Developers list:
  <a href="mailto:dev@calcite.apache.org">dev at calcite.apache.org</a>
  (<a href="http://mail-archives.apache.org/mod_mbox/calcite-dev/">archive</a>,
  <a href="mailto:dev-subscribe@calcite.apache.org">subscribe</a>)
* Issues: https://github.com/Stream-SQL-TCK/Stream-SQL-TCK/issues
* <a href="HISTORY.md">Release notes and history</a>
* <a href="HOWTO.md">HOWTO</a>
