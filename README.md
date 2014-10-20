Thrift Node.js Library
=========================

NOTE: This is a copy (not a fork) of the current (as of October 20 2014) state of https://github.com/apache/thrift node.js library.

It's only purpose is because I need the 0.9.2 code in an app but Apache Thrift have not released it after 3 months as a release candidate. It fixes a major issue where TCP connections throw unhandleable errors if they ever disconnect unexpectedly (which is comon occurence in real life). Npm also has no way to refer to a module that is not at the root of the repo by commitish so I needed top copy just the node part into a new repo just to get it to install with NPM.

This does NOT add any changes and may NOT keep in-sync with upstream. If you really need Thrift server for node.js and 0.9.2 still isn't out you might use this temporarily but no warranty etc...

## Original README:

License
-------
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements. See the NOTICE file
distributed with this work for additional information
regarding copyright ownership. The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License. You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied. See the License for the
specific language governing permissions and limitations
under the License.


## Install

    npm install thrift 

## Thrift Compiler

You can compile IDL sources for Node.js with the following command:

    thrift --gen js:node thrift_file

## Cassandra Client Example:

Here is a Cassandra example:

    var thrift = require('thrift'),
        Cassandra = require('./gen-nodejs/Cassandra')
        ttypes = require('./gen-nodejs/cassandra_types');

    var connection = thrift.createConnection("localhost", 9160),
        client = thrift.createClient(Cassandra, connection);

    connection.on('error', function(err) {
      console.error(err);
    });

    client.get_slice("Keyspace", "key", new ttypes.ColumnParent({column_family: "ExampleCF"}), new ttypes.SlicePredicate({slice_range: new ttypes.SliceRange({start: '', finish: ''})}), ttypes.ConsistencyLevel.ONE, function(err, data) {
      if (err) {
        // handle err
      } else {
        // data == [ttypes.ColumnOrSuperColumn, ...]
      }
      connection.end();
    });

<a name="int64"></a>
## Int64

Since JavaScript represents all numbers as doubles, int64 values cannot be accurately represented naturally. To solve this, int64 values in responses will be wrapped with Thirft.Int64 objects. The Int64 implementation used is [broofa/node-int64](https://github.com/broofa/node-int64).

## Libraries using node-thrift

* [yukim/node_cassandra](https://github.com/yukim/node_cassandra)

## Custom client and server example

An example based on the one shown on the Thrift front page is included in the examples/ folder.
