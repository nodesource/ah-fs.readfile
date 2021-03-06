# ah-fs.processor [![build status](https://secure.travis-ci.org/nodesource/ah-fs.processor.png)](http://travis-ci.org/nodesource/ah-fs.processor)

Processes ah-fs data obtained from async resources related to file system opearations.

## Installation

    npm install ah-fs.processor

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

-   [API](#api)
    -   [ReadFileProcessor](#readfileprocessor)
    -   [readFileProcessor.process](#readfileprocessorprocess)
    -   [Groups](#groups)
    -   [Operations](#operations)
        -   [`fs.readFile` specific Operation Properties](#fsreadfile-specific-operation-properties)
        -   [General Operation Properties](#general-operation-properties)
    -   [Sample Return Value](#sample-return-value)
    -   [ReadFileProcessor.operationSteps](#readfileprocessoroperationsteps)
    -   [ReadFileProcessor.operation.null](#readfileprocessoroperationnull)
    -   [ReadFileOperation](#readfileoperation)
    -   [readFileOperation.\_processOpen](#readfileoperation%5C_processopen)
    -   [readFileOperation.\_processStat](#readfileoperation%5C_processstat)
    -   [readFileOperation.\_processRead](#readfileoperation%5C_processread)
    -   [readFileOperation.\_processClose](#readfileoperation%5C_processclose)
    -   [readFileOperation.summary](#readfileoperationsummary)
    -   [Properties Specific to `fs.readFile`](#properties-specific-to-fsreadfile)
    -   [ReadStreamProcessor](#readstreamprocessor)
    -   [readStreamProcessor.process](#readstreamprocessorprocess)
    -   [Groups](#groups-1)
    -   [Operations](#operations-1)
        -   [`fs.createReadStream` specific Operation Properties](#fscreatereadstream-specific-operation-properties)
        -   [General Operation Properties](#general-operation-properties-1)
    -   [Sample Return Value](#sample-return-value-1)
    -   [ReadStreamProcessor.operationSteps](#readstreamprocessoroperationsteps)
    -   [ReadStreamProcessor.operation.null](#readstreamprocessoroperationnull)
    -   [ReadStreamOperation](#readstreamoperation)
    -   [readStreamOperation.\_processOpen](#readstreamoperation%5C_processopen)
    -   [readStreamOperation.\_processTick](#readstreamoperation%5C_processtick)
    -   [readStreamOperation.\_processRead](#readstreamoperation%5C_processread)
    -   [readStreamOperation.\_processClose](#readstreamoperation%5C_processclose)
    -   [readStreamOperation.summary](#readstreamoperationsummary)
    -   [Properties Specific to `fs.createReadStream`](#properties-specific-to-fscreatereadstream)
    -   [openInitFrame0Rx](#openinitframe0rx)
    -   [writeInitFrame0Rx](#writeinitframe0rx)
    -   [closeInitFrame0Rx](#closeinitframe0rx)
    -   [WriteFileProcessor](#writefileprocessor)
    -   [writeFileProcessor.process](#writefileprocessorprocess)
    -   [Groups](#groups-2)
    -   [Operations](#operations-2)
        -   [`fs.createWriteFile` specific Operation Properties](#fscreatewritefile-specific-operation-properties)
        -   [General Operation Properties](#general-operation-properties-2)
    -   [Sample Return Value](#sample-return-value-2)
    -   [WriteFileProcessor.operationSteps](#writefileprocessoroperationsteps)
    -   [WriteFileProcessor.operation.null](#writefileprocessoroperationnull)
    -   [WriteFileOperation](#writefileoperation)
    -   [writeFileOperation.\_processOpen](#writefileoperation%5C_processopen)
    -   [writeFileOperation.\_processWrite](#writefileoperation%5C_processwrite)
    -   [writeFileOperation.\_processClose](#writefileoperation%5C_processclose)
    -   [writeFileOperation.summary](#writefileoperationsummary)
    -   [Properties Specific to `fs.writeFile`](#properties-specific-to-fswritefile)
    -   [WriteStreamProcessor](#writestreamprocessor)
    -   [writeStreamProcessor.process](#writestreamprocessorprocess)
    -   [Groups](#groups-3)
    -   [Operations](#operations-3)
        -   [`fs.createWriteStream` specific Operation Properties](#fscreatewritestream-specific-operation-properties)
        -   [General Operation Properties](#general-operation-properties-3)
    -   [Sample Return Value](#sample-return-value-3)
    -   [writeStreamProcessor.\_separteIntoGroups](#writestreamprocessor%5C_separteintogroups)
    -   [Connecting WriteSteam Write to WriteStream Close](#connecting-writesteam-write-to-writestream-close)
    -   [Connecting WriteStream Open to WriteStream Write](#connecting-writestream-open-to-writestream-write)
    -   [WriteStreamProcessor.operationSteps](#writestreamprocessoroperationsteps)
    -   [WriteStreamProcessor.operation.null](#writestreamprocessoroperationnull)
    -   [WriteStreamOperation](#writestreamoperation)
    -   [writeStreamOperation.\_processOpen](#writestreamoperation%5C_processopen)
    -   [writeStreamOperation.\_processTick](#writestreamoperation%5C_processtick)
    -   [writeStreamOperation.\_processwrite](#writestreamoperation%5C_processwrite)
    -   [writeStreamOperation.\_processClose](#writestreamoperation%5C_processclose)
-   [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## [API](https://nodesource.github.io/ah-fs.processor)

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### processFileSystem

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `$0.activities` **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String), [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)>** a map of async activities hashed by id
    -   `$0.includeActivities` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the actual activities are appended to the output (optional, default `false`)

### ReadFileProcessor

Instantiates an fs.readFile data processor to process data collected via
[nodesource/ah-fs](https://github.com/nodesource/ah-fs)

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
-   `includeActivities` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the actual activities are appended to the output (optional, default `false`)

### readFileProcessor.process

Processes the supplied async activities and splits them into
groups, and operations each representing a file read `fs.readFile`.

### Groups

The returned value has a `groups` property which just lists the ids
of async resources that were grouped together to form an operation
indexed by the id of the `open` resource.
Thus the `groups` is a map of sets.
If no file read was encountered the groups are empty.

### Operations

Additionally an `operations` property is included as well. Each operation
represents one full `fs.readFile` execution. There will be one operation per
group and they are indexed by the corresponding open resource `id` as well.

An `operation` has the following properties:

#### `fs.readFile` specific Operation Properties

 Data about the async resources that were part of the operation, by default
 only `id` and `triggerId` are included:

-   **open**: contains data about opening the file
-   **stat**: contains data about getting file stats
-   **read**: contains data about reading the file
-   **close**: contains data about closing the file

#### General Operation Properties

The information below is the same for all `operation`s and thus is only
mentioned here and linked from the documentation of all other processors.

 Data about the lifetime of the operation:

-   **lifeCycle**: contains three timestamps that detail when an operation was created,
    for how long it was alive and when it was destroyed.

    -   **created**: the timestamp when the first resource of the operation was created
    -   **destroyed**: the timestamp when the last resource of the operation was destroyed
    -   **timeAlive**: the difference between the `destroyed` and `created` timestamps, i.e.
        how long the operation's resources were alive

    Each timestamp has the following two properties provided by [utils.prettyNs](#utilsprettyns).

    -   **ns**: time in nanoseconds {Number}
    -   **ms**: pretty printed time in milliseconds {String}

Data that links to user code that is responsible for the operation occurring.

-   **createdAt**: provides the line of code that called `fs.readFile`
-   **userFunctions**: depending on the settings (see constructor docs) each resource
    will include it's own array of userFunctions or they are separated out into
    one property with duplicates merged. The latter is the default behavior.
    In either case `userFunctions` is an Array of Objects with the following properties:

    -   **name**: the function name
    -   **inferredName**: the inferred function name, only needed if the `name` is not set
    -   **file**: the file in which the function was defined
    -   **line**: the line on which the functino was defined in that file
    -   **column**: the column on which the functino was defined in that file
    -   **location**: the file and line + column where the function was defined combined into a string
    -   **args**: the `err` and information about the `res` of the operation
        with which the function was invoked
    -   **propertyPaths**: the object paths at which the function was found, these could be multiple
        since the function could've been attached to multiple resources (only available if the functions
        were separated from the resources and merged)
    -   **propertyPath**: the object path at which the function was found (only available if the
        functions weren't separated and thus are still part of each resource)

### Sample Return Value

The sample return value was created with default options.

```js
{ groups: Map { 10 => Set { 10, 11, 12, 13 } },
  operations:
    Map {
      10 => { lifeCycle:
        { created: { ms: '44.12ms', ns: 44119000 },
          destroyed: { ms: '85.95ms', ns: 85955000 },
          timeAlive: { ms: '41.84ms', ns: 41836000 } },
      createdAt: 'at Test.<anonymous> (/Volumes/d/dev/js/async-hooks/ah-fs/test/read-one-file.js:36:6)',
      open: { id: 10, triggerId: 1 },
      stat: { id: 11, triggerId: 10 },
      read: { id: 12, triggerId: 11 },
      close: { id: 13, triggerId: 12 },
      userFunctions:
        [ { file: '/Volumes/d/dev/js/async-hooks/ah-fs/test/read-one-file.js',
            line: 39,
            column: 17,
            inferredName: '',
            name: 'onread',
            location: 'onread (/Volumes/d/dev/js/async-hooks/ah-fs/test/read-one-file.js:39:17)',
            args:
            { '0': null,
              '1':
                { type: 'Buffer',
                  len: 6108,
                  included: 18,
                  val:
                  { utf8: 'const test = requi',
                    hex: '636f6e73742074657374203d207265717569' } },
              proto: 'Object' },
            propertyPaths:
            [ 'open.resource.context.callback',
              'stat.resource.context.callback',
              'read.resource.context.callback',
              'close.resource.context.callback' ] } ] } } }
```

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about `fs.readFile` operations with the
structure outlined above

### ReadFileProcessor.operationSteps

The minimum number of steps, represented as an async resource each,
involved to execute `fs.readFile`.

This can be used by higher level processors to group
activities looking for larger operations first and then
operations involving less steps.

Steps are: open, stat, read+, close

### ReadFileProcessor.operation

Description of the operation: 'fs.readFile'.

### ReadFileOperation

Processes a group of async activities that represent a fs read stream operation.
It is used by the [ReadFileProcessor](#readfileprocessor) as part of `process`.

Four operation steps are derived from the group, each providing some information
about the operation in question.

Each step is processed into an operation in the corresponding private method, i.e. `_processOpen`.
These methods are documented below for information's sake, they should not be called directly,
nor should you have a need to directly instantiate a `ReadFileOperation` in the first place.

**Parameters**

-   `group` **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)>>** the ids of the activities that were part of the operation
-   `includeActivities` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the activities are attached to
    each operation step (optional, default `false`)

### readFileOperation.\_processOpen

The open resource tells us where in user code the `fs.readFile` originated
via the second frame of the stack trace, as well as when the operation
was created.

Additionally it has the same user functions attached as all the other resources.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the open step, pre-processed by the `ReadFileProcessor`.

### readFileOperation.\_processStat

The stat resource gives us no interesting information.
Therefore we just capture the `id`, `triggerId` and `userFunctions` and if so desired
attach the activities.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the open step, pre-processed by the `ReadFileProcessor`.

### readFileOperation.\_processRead

The read resource gives us no interesting information.
Therefore we just capture the `id`, `triggerId` and `userFunctions` and if so desired
attach the activities.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the read step, pre-processed by the `ReadFileProcessor`.

### readFileOperation.\_processClose

The main information we pull from the close resource is the `destroy` timestamp.

Combined with the `init` timestamp of the open resource it allows us to deduce how long
the file read took.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the close step, pre-processed by the `ReadFileProcessor`.

### readFileOperation.summary

Returns the summary of processing the group into an operation.

The summary of all operations has a very similar structure, but includes some properties that are specific to this
particular operation.

The general properties `lifeCycle` and `createdAt` are documented as part of
the `ReadFileProcessor`.
Therefore learn more [here](#general-operation-properties).

### Properties Specific to `fs.readFile`

-   **open**: see `readFileOperation._processOpen`
-   **stat**: see `readFileOperation._processStat`
-   **read**: see `readFileOperation._processRead`
-   **close**: see `readFileOperation._processClose`

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** options
    -   `$0.separateFunctions` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** when `true` the user functions are separated out
        from the specific operations and attached as a `userFunctions` array directly to the returned
        result (optional, default `true`)
    -   `$0.mergeFunctions` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` when a duplicate function is found in the
        separated functions Array, they are merged into one while preserving all information
        from both version. Note that this setting only activates if `separateFunctions` is `true` as well. (optional, default `true`)

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** all important information about the current operation

### ReadStreamProcessor

Instantiates an fs.createReadStream data processor to process data collected via
[nodesource/ah-fs](https://github.com/nodesource/ah-fs)

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
-   `includeActivities` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the actual activities are appended to the output (optional, default `false`)

### readStreamProcessor.process

Processes the supplied async activities and splits them into
groups, and operations each representing a file read stream `fs.createReadStream`.

### Groups

The returned value has a `groups` property which just lists the ids
of async resources that were grouped together to form an operation
indexed by the `fd` on which the readFile operated.
Thus the `groups` is a map of sets.
If no file read stream was encountered the groups are empty.

### Operations

Additionally an `operations` property is included as well. Each operation
represents one full `fs.createReadStream` execution. There will be one operation per
group and they are indexed by the corresponding `fd` as well.

An `operation` has the following properties:

#### `fs.createReadStream` specific Operation Properties

 Data about the async resources that were part of the operation, by default
 only `id` and `triggerId` are included:

-   **open**: contains data about opening the file
-   **stream**: contains data about how the stream was configured, including readable state and
    the path to the file being read, pipes count, encoding, etc.
-   **reads**: an Array of reads, each containing data about reading a chunk from the file including
    the time spent to complete reading the particular chunk
-   **close**: contains data about closing the file

#### General Operation Properties

-   [see ReadFileProcessor.process](https://nodesource.github.io/ah-fs.processor/#general-operation-properties)

### Sample Return Value

The sample return value was created with default options.

```js
{ groups: Map { 10 => Set { 10, 12, 13, 14, 16 } },
  operations:
  Map {
    10 => { lifeCycle:
      { created: { ms: '1.60ms', ns: 1600000 },
        destroyed: { ms: '14.33ms', ns: 14329000 },
        timeAlive: { ms: '12.73ms', ns: 12729000 } },
    createdAt: 'at Test.<anonymous> (/Volumes/d/dev/js/async-hooks/ah-fs/test/readstream-one-file.js:94:6)',
    open: { id: 10, triggerId: 3 },
    stream:
      { id: 14,
        triggerId: 12,
        path: '/Volumes/d/dev/js/async-hooks/ah-fs/test/readstream-one-file.js',
        flags: 'r',
        fd: 19,
        objectMode: false,
        highWaterMark: 65536,
        pipesCount: 0,
        defaultEncoding: 'utf8',
        encoding: null },
    reads:
      [ { id: 12,
          triggerId: 10,
          timeSpent: { ms: '0.83ms', ns: 830000 } },
        { id: 13,
          triggerId: 12,
          timeSpent: { ms: '0.24ms', ns: 240000 } } ],
    close: { id: 16, triggerId: 13 },
    userFunctions:
      [ { file: '/Volumes/d/dev/js/async-hooks/ah-fs/test/readstream-one-file.js',
          line: 99,
          column: 16,
          inferredName: '',
          name: 'onend',
          location: 'onend (/Volumes/d/dev/js/async-hooks/ah-fs/test/readstream-one-file.js:99:16)',
          args: null,
          propertyPaths: [ 'stream.resource.args[0]._events.end[1]' ] },
        { file: '/Volumes/d/dev/js/async-hooks/ah-fs/test/readstream-one-file.js',
          line: 98,
          column: 17,
          inferredName: '',
          name: 'ondata',
          location: 'ondata (/Volumes/d/dev/js/async-hooks/ah-fs/test/readstream-one-file.js:98:17)',
          args: null,
          propertyPaths: [ 'stream.resource.args[0]._events.data' ] } ] } } }
```

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about `fs.createReadStream` operations with the
structure outlined above

### ReadStreamProcessor.operationSteps

The minimum number of steps, represented as an async resource each,
involved to execute `fs.createReadStream`.

This can be used by higher level processors to group
activities looking for larger operations first and then
operations involving less steps.

Steps are: open, stream+, read+, close

### ReadStreamProcessor.operation.null

Description of the operation: 'fs.createReadStream'.

### ReadStreamOperation

Processes a group of async activities that represent a fs read stream operation.
It is used by the [ReadStreamProcessor](#readstreamprocessor) as part of `process`.

Four operation steps are derived from the group, each providing some information
about the operation in question.

Each step is processed into an operation in the corresponding private method, i.e. `_processOpen`.
These methods are documented below for information's sake, they should not be called directly,
nor should you have a need to directly instantiate a `ReadStreamOperation` in the first place.

**Parameters**

-   `group` **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)>>** the ids of the activities that were part of the operation
-   `includeActivities` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the activities are attached to
    each operation step (optional, default `false`)

### readStreamOperation.\_processOpen

An open doesn't have too much info, but we can glean two very important
 data points:

1.  the init timestamp tells us when the stream was created
2.  the last frame of the init stack tells us where `createReadStream` was called.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the open step, pre-processed by the `ReadStreamProcessor`.

### readStreamOperation.\_processTick

The ReadStream Tick gives us a lot of information. It has an args array with
the ReadStream and its ReadableState respectively

The ReadStream provides us the following:

1.  the path to the file we are streaming
2.  the flags with which the file was opened
3.  the fd (assuming we are dealing with the tick triggered indirectly by the open)

All callbacks on the \_events of the ReadStream have been removed, but are present
inside the functions object (see below).

The ReadableState provides us the following:

1.  objectMode `true|false`
2.  highWaterMark
3.  pipesCount
4.  defaultEncoding, i.e. utf8
5.  encoding, i.e. utf8

The information extracted from the tick is attached to a `stream` property
provided with the `summary`.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the tick step, pre-processed by the `ReadStreamProcessor`.

### readStreamOperation.\_processRead

The read resource doesn't give us too much information.

The stack traces originate in core and we don't see any registred
user callbacks, as those are present on the stream instead.
However we can count the amount of reads that occurred and deduce how
long each read took from the `before` and `after` timestamps.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the read step, pre-processed by the `ReadStreamProcessor`.

### readStreamOperation.\_processClose

The main information we pull from the close resource is the `destroy` timestamp.

Combined with the `init` timestamp of the open resource it allows us to deduce how long
the read stream was active.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the close step, pre-processed by the `ReadStreamProcessor`.

### readStreamOperation.summary

Returns the summary of processing the group into an operation.

The summary of all operations has a very similar structure, but includes some properties that are specific to this
particular operation.

The general properties `lifeCycle` and `createdAt` are documented as part of
the `ReadFileProcessor`.
Therefore learn more [here](#general-operation-properties).

### Properties Specific to `fs.createReadStream`

-   **open**: see `readStreamOperation._processOpen`
-   **stream**: see `readStreamOperation._processTick`
-   **read**: see `readStreamOperation._processRead`
-   **close**: see `readStreamOperation._processClose`

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** options
    -   `$0.separateFunctions` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** when `true` the user functions are separated out
        from the specific operations and attached as a `userFunctions` array directly to the returned
        result (optional, default `true`)
    -   `$0.mergeFunctions` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` when a duplicate function is found in the
        separated functions Array, they are merged into one while preserving all information
        from both version. Note that this setting only activates if `separateFunctions` is `true` as well. (optional, default `true`)

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** all important information about the current operation

### openInitFrame0Rx

Sample initStack of writeFile open, calles as first operation of `fs.writeFile`.
In order to be sure this is a writeFile open we need to check the two topmost frames.

"at Object.fs.open (fs.js:581:11)",
"at Object.fs.writeFile (fs.js:1155:6)",
"at Test.<anonymous> (/Volumes/d/dev/js/async-hooks/ah-fs/test/write-one-file.js:28:6)",

Code at fs.js:581:

`binding.open(pathModule._makeLong(path), ...`

Code at fs.js:1155:

`fs.open(path, flag, options.mode, function(openErr, fd) ...`

Bottom frame has info about where the call `fs.writeFile` originated.

### writeInitFrame0Rx

Sample init stack of writeFile write, called afer `fs.open` completes:

"at Object.fs.write (fs.js:643:20)",
"at writeAll (fs.js:1117:6)",
"at writeFd (fs.js:1168:5)",
"at fs.js:1159:7",
"at FSReqWrap.oncomplete (fs.js:117:15)"

Code at fs.js:643:

`binding.writeBuffer(fd, buffer, offset, length, position, req);`

### closeInitFrame0Rx

Sample initStack of writeFile close, called after last `fs.write` completes:

"at Object.fs.close (fs.js:555:11)",
"at fs.js:1131:14",
"at FSReqWrap.wrapper [as oncomplete](fs.js:626:5)"

Code at fs.js:555:

`binding.close(fd, req);`

### WriteFileProcessor

Instantiates an fs.writeFile data processor to process data collected via
[nodesource/ah-fs](https://github.com/nodesource/ah-fs)

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
-   `includeActivities` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the actual activities are appended to the output (optional, default `false`)

### writeFileProcessor.process

Processes the supplied async activities and splits them into
groups, and operations each representing a file read stream `fs.createWriteFile`.

### Groups

The returned value has a `groups` property which just lists the ids
of async resources that were grouped together to form an operation
indexed by the id of the `fs.open` activity that was part of the `fs.writeFile`.
Thus the `groups` is a map of sets.
If no file write file was encountered the groups are empty.

### Operations

Additionally an `operations` property is included as well. Each operation
represents one full `fs.writeFile` execution. There will be one operation per
group and they are indexed by the corresponding open id as well.

An `operation` has the following properties:

#### `fs.createWriteFile` specific Operation Properties

 Data about the async resources that were part of the operation, by default
 only `id` and `triggerId` are included:

-   **open**: contains data about opening the file
-   **writes**: an Array of writes, each containing data about writing a
    chunk from the file including the time spent to complete writing the
    particular chunk
-   **close**: contains data about closing the file

#### General Operation Properties

-   [see ReadFileProcessor.process](https://nodesource.github.io/ah-fs.processor/#general-operation-properties)

### Sample Return Value

The sample return value was created with default options.

```js
{ groups: Map { 10 => Set { 10, 11, 12 } },
  operations:
  Map {
    10 => { lifeCycle:
      { created: { ms: '24.49ms', ns: 24491000 },
        destroyed: { ms: '33.96ms', ns: 33964000 },
        timeAlive: { ms: '9.47ms', ns: 9473000 } },
    createdAt: 'at Test.<anonymous> (/Volumes/d/dev/js/async-hooks/ah-fs/test/write-one-file.js:28:6)',
    open: { id: 10, triggerId: 1 },
    write: { id: 11, triggerId: 10 },
    close: { id: 12, triggerId: 11 } } } }
```

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about `fs.createWriteFile` operations with the
structure outlined above

### WriteFileProcessor.operationSteps

The minimum number of steps, represented as an async resource each,
involved to execute `fs.writeFile`.

This can be used by higher level processors to group
activities looking for larger operations first and then
operations involving less steps.

Steps are: open, write+, close

### WriteFileProcessor.operation.null

Description of the operation: 'fs.writeFile'.

### WriteFileOperation

Processes a group of async activities that represent a fs write file operation.
It is used by the [WriteFileProcessor](#WriteFileProcessor) as part of `process`.

Three operation steps are derived from the group, each providing some information
about the operation in question.

Each step is processed into an operation in the corresponding private method, i.e. `_processOpen`.
These methods are documented below for information's sake, they should not be called directly,
nor should you have a need to directly instantiate a `WriteFileOperation` in the first place.

**Parameters**

-   `group` **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)>>** the ids of the activities that were part of the operation
-   `includeActivities` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the activities are attached to
    each operation step (optional, default `false`)

### writeFileOperation.\_processOpen

The open resource tells us where in user code the `fs.writeFile` originated
via the second frame of the stack trace, as well as when the operation
was created.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the open step, pre-processed by the `WriteFileProcessor`.

### writeFileOperation.\_processWrite

The write resource gives us no interesting information.
Therefore we just capture the `id`, `triggerId` and if so desired
attach the activities.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the write step, pre-processed by the `WriteFileProcessor`.

### writeFileOperation.\_processClose

The main information we pull from the close resource is the `destroy` timestamp.

Combined with the `init` timestamp of the open resource it allows us to deduce how long
the file write took.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the close step, pre-processed by the `WriteFileProcessor`.

### writeFileOperation.summary

Returns the summary of processing the group into an operation.

The summary of all operations has a very similar structure, but includes some properties that are specific to this
particular operation.

The general properties `lifeCycle` and `createdAt` are documented as part of
the `WriteFileProcessor`.
Therefore learn more [here](#general-operation-properties).

### Properties Specific to `fs.writeFile`

-   **open**: see `writeFileOperation._processOpen`
-   **write**: see `writeFileOperation._processWrite`
-   **close**: see `writeFileOperation._processClose`

Note this summary function takes no parameters (like the other Operations) since
we don't find any user functions related to the write file operation and thus
have nothing to process.

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** all important information about the current operation

### WriteStreamProcessor

Instantiates an fs.createWriteStream data processor to process data collected via
[nodesource/ah-fs](https://github.com/nodesource/ah-fs)

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `$0.includeActivities` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the actual activities are appended to the output (optional, default `false`)
    -   `$0.separateFunctions` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** when `true` the user functions are separated out
        from the specific resources and attached as a `userFunctions` array directly to the returned
        operations (optional, default `true`)

### writeStreamProcessor.process

Processes the supplied async activities and splits them into
groups, and operations each representing a file write stream `fs.createWriteStream`.

### Groups

The returned value has a `groups` property which just lists the ids
of async resources that were grouped together to form an operation
indexed by the `fd` on which the writeFile operated.
Thus the `groups` is a map of sets.
If no file write stream was encountered the groups are empty.

### Operations

Additionally an `operations` property is included as well. Each operation
represents one full `fs.createWriteStream` execution. There will be one operation per
group and they are indexed by the corresponding `fd` as well.

An `operation` has the following properties:

#### `fs.createWriteStream` specific Operation Properties

 Data about the async resources that were part of the operation, by default
 only `id` and `triggerId` are included:

-   **open**: contains data about opening the file
-   **stream**: contains data about how the stream was configured, including writeable state and
    the path to the file being write, pipes count, encoding, etc.
-   **writes**: an Array of writes, each containing data about writing a chunk from the file including
    the time spent to complete writeing the particular chunk
-   **close**: contains data about closing the file

#### General Operation Properties

-   [see ReadFileProcessor.process](https://nodesource.github.io/ah-fs.processor/#general-operation-properties)

### Sample Return Value

The sample return value was created with default options.

```js
{ groups: Map { 10 => Set { 14, 10, 16, 19 } },
  operations:
  Map {
    10 => { lifeCycle:
      { created: { ms: '1.12ms', ns: 1123000 },
        destroyed: { ms: '18.20ms', ns: 18205000 },
        timeAlive: { ms: '17.08ms', ns: 17082000 } },
    createdAt: 'at Test.<anonymous> (/Volumes/d/dev/js/async-hooks/ah-fs/test/read-stream-piped-into-write-stream.js:29:26)',
    open: { id: 10, triggerId: 3 },
    stream:
      { id: 16,
        triggerId: 13,
        path: '/dev/null',
        flags: 'w',
        fd: 19,
        mode: 438 },
    writes:
      [ { id: 14,
          triggerId: 13,
          timeSpent: { ms: '0.14ms', ns: 139000 } } ],
    close: { id: 19, triggerId: 15 },
    userFunctions:
      [ { file: '/Volumes/d/dev/js/async-hooks/ah-fs/test/read-stream-piped-into-write-stream.js',
          line: 32,
          column: 19,
          inferredName: '',
          name: 'onfinish',
          location: 'onfinish (/Volumes/d/dev/js/async-hooks/ah-fs/test/read-stream-piped-into-write-stream.js:32:19)',
          args: null,
          propertyPaths: [ 'stream.resource.args[1].pipes._events.finish[1]' ] } ] } } }
```

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about `fs.createWriteStream` operations with the
structure outlined above

### writeStreamProcessor.\_separteIntoGroups

Here we try our best to piece together the parts of a WriteStream,
Open | Write+ | WriteStreamTick | Close.

Since they aren't linked by a common file descriptor or similar we rely
on async resource graph structure and the timestamps to take a best guess.

We just don't have the data available to piece this together with 100% certainty.

Below is a sample of collected async resources with all but types and ids removed.

     { type: 'FSREQWRAP', id: 10, tid: 3 }   open, write stream triggered by root
     { type: 'FSREQWRAP', id: 11, tid: 3 }   open, read stream triggered by root
     { type: 'TickObject', id: 12, tid: 3 }  read stream tick, triggered by root
     { type: 'FSREQWRAP', id: 13, tid: 11 }  read, triggerd by open of read steam
     { type: 'FSREQWRAP', id: 14, tid: 13 }  write, triggerd by read of read steam
     { type: 'FSREQWRAP', id: 15, tid: 13 }  read, next chunk, triggered by first read
     { type: 'TickObject', id: 16, tid: 13 } stream tick, triggerd by first read
     { type: 'FSREQWRAP', id: 18, tid: 15 }  close read stream, triggered by last read
     { type: 'FSREQWRAP', id: 19, tid: 15 }  close write stream, triggered by last read

We reason about that data as follows in order to piece together the WriteStream.

### Connecting WriteSteam Write to WriteStream Close

Write (id: 14) is triggered by read of read stream (id: 13).
The same read triggers the last read (id: 15).
That last read triggers the close of the write stream (id: 19).

Therefore we can connect the write stream write to the write stream close since they
have a common parent in their ancestry (the first read of the read stream).

               -- Read2:15 -- WriteStream:Close:19
             /
    Read1:13
             \
               -- WriteStream:Write:14

However I would imagine that this breaks down once we have on read stream piped into multiple
write streams as then the writes have the same Read parent.

### Connecting WriteStream Open to WriteStream Write

There is no 100% way to get this right, but if we assume that the first write happens right after
the opening of the write stream in the same context we can do the following.

We already know that the common parent of WriteStream:Write and
WriteStream:Close is Read1:13.
Therefore we find all WriteStream:Opens that share a parent with Read1:13.
The ones with the closest parent win.

If we find more than one, we pick the one that was initialized closest to
the WriteStream:Write timewise, assuming that we write to the stream
immediately after opening it.

               -- ReadStream:Open:11 -- Read1:13 -- Read2:15 -- WriteStream:Close:19
             /                                   \
    Parent:3                                       -- WriteStream:Write:14
             \
               -- WriteStream:Open:10

### WriteStreamProcessor.operationSteps

The minimum number of steps, represented as an async resource each,
involved to execute `fs.createWriteStream`.

This can be used by higher level processors to group
activities looking for larger operations first and then
operations involving less steps.

Steps are: open, stream, write+, close

### WriteStreamProcessor.operation.null

Description of the operation: 'fs.createWriteStream'.

### WriteStreamOperation

Processes a group of async activities that represent a fs write stream operation.
It is used by the [writeStreamProcessor](#writestreamprocessor) as part of `process`.

Four operation steps are derived from the group, each providing some information
about the operation in question.

Each step is processed into an operation in the corresponding private method, i.e. `_processOpen`.
These methods are documented below for information's sake, they should not be called directly,
nor should you have a need to directly instantiate a `writeStreamOperation` in the first place.

**Parameters**

-   `group` **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)&lt;[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)>>** the ids of the activities that were part of the operation
-   `includeActivities` **[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if `true` the activities are attached to
    each operation step (optional, default `false`)

### writeStreamOperation.\_processOpen

An open doesn't have too much info, but we can glean two very important
 data points:

1.  the init timestamp tells us when the stream was created
2.  the last frame of the init stack tells us where `createWriteStream` was called.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the open step, pre-processed by the `WriteStreamProcessor`.

### writeStreamOperation.\_processTick

The WriteStream Tick gives us a lot of information. It is the same tick object that we process
in the ReadStreamOperation to glean data about the read stream.
It has an args array with the ReadStream and its ReadableState respectively.

The ReadableState included the WritableState which the [ah-fs](https://github.com/nodesource/ah-fs) pre-processor
already plucked for us and added as the 3rd argument.
Additionally it includes lots of functions including user functions registered with the
WriteStream, i.e. `on('finish')`.

Ergo the WriteStream provides us the following as part of the WritableState:

1.  the path to the file we are writing into
2.  the flags with which the file was opened
3.  the fd (assuming we are dealing with the tick triggered indirectly by the open)

All callbacks on the \_events of the ReadStream and WriteStream have been removed, but are present
inside the functions object (see below).

The information extracted from the tick is attached to a `stream` property
provided with the `summary`.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the tick step, pre-processed by the `WriteStreamProcessor`.

### writeStreamOperation.\_processwrite

The write resource doesn't give us too much information.

The stack traces originate in core and we don't see any registred
user callbacks, as those are present on the stream instead.
However we can count the amount of writes that occurred and deduce how
long each write took from the `before` and `after` timestamps.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the write step, pre-processed by the `WriteStreamProcessor`.

### writeStreamOperation.\_processClose

The main information we pull from the close resource is the `destroy` timestamp.

Combined with the `init` timestamp of the open resource it allows us to deduce how long
the write stream was active.

**Parameters**

-   `info` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** information about the close step, pre-processed by the `WriteStreamProcessor`.

## License

MIT
