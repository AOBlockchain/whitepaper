# Technical Background

AO Blockchain System is designed to run on cloud providers that support the Kubernetes container orchestration platform. The system itself is coded in NodeJS and makes use of the newest ‘Serverless’ computing techniques to enhance its ability to process transactions. The Application Nodes autoscale based on CPU load. The Delegate node autoscales up to the available capacity using Kubeless, an open-source, Kubernetes-based Function service, running on Kafka.

## Kubernetes

"[Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is an open-source system for automating deployment, scaling, and management of containerized applications.

It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon [15 years of experience of running production workloads at Google](http://queue.acm.org/detail.cfm?id=2898444), combined with best-of-breed ideas and practices from the community." 

- Kubernetes Website

Kubernetes was selected as a base due to its ability to scale, support on-cloud hosting services, and being an auditable open-source project. This keeps the entire infrastructure of the AO Blockchain System in the public.

## Javascript

"JavaScript (JS) is a lightweight interpreted or JIT-compiled programming language with [first-class functions](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function). While it is most well-known as the scripting language for Web pages, [many non-browser environments](https://en.wikipedia.org/wiki/JavaScript#Uses_outside_Web_pages) also use it, such as [Node.js](https://developer.mozilla.org/en-US/docs/Glossary/Node.js), [Apache CouchDB](https://couchdb.apache.org/) and [Adobe Acrobat](http://www.adobe.com/devnet/acrobat/javascript.html). JavaScript is a [prototype-based](https://developer.mozilla.org/en-US/docs/Glossary/Prototype-based_programming), multi-paradigm, dynamic language, supporting object-oriented, imperative, and declarative (e.g. functional programming) styles." 

- Mozilla Developer Network

Javascript was selected due to its ubiquitous nature in programming, and its ability to run on both the server side, and client side. This makes audits easier as code between client and server will be similar. It also enables the AO Blockchain System to allow complex functions to be handled in the client in the same way it is handled on a server, enabling transparency and requiring less trust by users.

[ECMAScript 2016+](http://kangax.github.io/compat-table/es2016plus) will be used for all new code. Packages that are used that do not support ECMAScript 2016+ will be updated to ECMAScript 2016+ as needed.

## NodeJS

"Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://developers.google.com/v8/). Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open-source libraries in the world." 

- NodeJS website

NodeJS is the best server-side javascript framework with great crypto-system support, such as the SHA3 NPM package and the OpenPGP NPM package. This, combined with tools like Browserify, enable the similarity between client and server code. Its ability to create robust, fast APIs will enable the expansion of client systems to things we have not yet foreseen.

[ECMAScript 2016+](http://kangax.github.io/compat-table/es2016plus) will be used exclusively. If a package that is needed is not written to ECMAScript 2016+ specifications, it will be forked, upgraded, and contributed back to the original project.

## MongoDB

"MongoDB is an open-source document database that provides high performance, high availability, and automatic scaling." 

- MongoDB Documentation

MongoDB is a fast, document database that stores data in a JSON-like format (BSON). In addition, it can be used as an in-memory datastore, as well as a persistent data store. This fulfills all of the needs of AO Blockchain System, from the transaction queue to the scalable, sharded block storage system. 

It is also very straightforward to install MongoDB on Kubernetes, as this will enable much easier management of an AONode.

## Database Structure (Documents)

### Blocks

<table>
  <tr>
    <td>Field Name</td>
    <td>Type</td>
    <td>Not Null</td>
  </tr>
  <tr>
    <td>id</td>
    <td>objectId</td>
    <td>true</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>timestamp</td>
    <td>true</td>
  </tr>
  <tr>
    <td>height</td>
    <td>int</td>
    <td>true</td>
  </tr>
  <tr>
    <td>previousBlock</td>
    <td>string</td>
    <td>false</td>
  </tr>
  <tr>
    <td>numOfTx</td>
    <td>int</td>
    <td>true</td>
  </tr>
  <tr>
    <td>totalAmounts</td>
    <td>object</td>
    <td>true</td>
  </tr>
  <tr>
    <td>totalFee</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkReward</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>payloadLength</td>
    <td>iint</td>
    <td>true</td>
  </tr>
  <tr>
    <td>payloadHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>delegatePKHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
</table>


### Currency Transactions (confirmed)

<table>
  <tr>
    <td>Field Name</td>
    <td>Type</td>
    <td>Not Null</td>
  </tr>
  <tr>
    <td>id</td>
    <td>objectId</td>
    <td>true</td>
  </tr>
  <tr>
    <td>type</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>timestamp</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderPkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderId</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>recipientId</td>
    <td>string</td>
    <td>false</td>
  </tr>
  <tr>
    <td>amount</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>fee</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>signatures</td>
    <td>array</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
</table>


### Asset Transactions (confirmed)

<table>
  <tr>
    <td>Column Name</td>
    <td>Type</td>
    <td>Not Null</td>
  </tr>
  <tr>
    <td>id</td>
    <td>objectId</td>
    <td>true</td>
  </tr>
  <tr>
    <td>type</td>
    <td>object</td>
    <td>true</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>timestamp</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderPublicKey</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderId</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>recipientId</td>
    <td>string</td>
    <td>false</td>
  </tr>
  <tr>
    <td>amount</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>fee</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>signatures</td>
    <td>array</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
</table>


### Share Transactions (confirmed)

<table>
  <tr>
    <td>Column Name</td>
    <td>Type</td>
    <td>Not Null</td>
  </tr>
  <tr>
    <td>id</td>
    <td>objectId</td>
    <td>true</td>
  </tr>
  <tr>
    <td>type</td>
    <td>object</td>
    <td>true</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>timestamp</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderPublicKey</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderId</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>recipientId</td>
    <td>string</td>
    <td>false</td>
  </tr>
  <tr>
    <td>amount</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>fee</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>signatures</td>
    <td>array</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
</table>


### Vote Transactions (confirmed)

<table>
  <tr>
    <td>Column Name</td>
    <td>Type</td>
    <td>Not Null</td>
  </tr>
  <tr>
    <td>id</td>
    <td>objectId</td>
    <td>true</td>
  </tr>
  <tr>
    <td>type</td>
    <td>object</td>
    <td>true</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>timestamp</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderPublicKey</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderId</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>recipientId</td>
    <td>string</td>
    <td>false</td>
  </tr>
  <tr>
    <td>amount</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>fee</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>signatures</td>
    <td>array</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
</table>


### Contract Transactions (confirmed)

<table>
  <tr>
    <td>Column Name</td>
    <td>Type</td>
    <td>Not Null</td>
  </tr>
  <tr>
    <td>id</td>
    <td>objectId</td>
    <td>true</td>
  </tr>
  <tr>
    <td>type</td>
    <td>object</td>
    <td>true</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>timestamp</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderPublicKey</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>senderId</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>recipientId</td>
    <td>string</td>
    <td>false</td>
  </tr>
  <tr>
    <td>amount</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>fee</td>
    <td>long</td>
    <td>true</td>
  </tr>
  <tr>
    <td>signatures</td>
    <td>array</td>
    <td>true</td>
  </tr>
  <tr>
    <td>blkHash</td>
    <td>string</td>
    <td>true</td>
  </tr>
  <tr>
    <td>chaincode</td>
    <td>string</td>
    <td>true</td>
  </tr>
</table>


Unconfirmed transactions reside in MongoDB in-memory queue.
