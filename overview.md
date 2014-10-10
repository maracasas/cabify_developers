---
layout: default
title: Cabify API Overview
---

Cabify API Overview
===================

This section describes the rules and general layout of the Cabify API. If you have any problems or requests, please contact our [development team](mailto:dev@cabify.com).

Current Version
---------------

The Cabify API is undergoing continual development and as such we do not currently require any version data. If at some point in the future we introduce non-backwards compatible changes we will look at providing new end points.

Schema
------

Access to the API is only permitted via HTTPS to the `cabify.com` domain. All data must be sent and received using JSON with the few notable exceptions of some OAuth authentication requests which accept regular www form data.

All requests to the api include the `/api` path. Any URL that does not include this is likely to be related to a web site that may be visited by a user.

Any properties that end in an `_at` suffix will be provided in ISO 8601 format in UTC including miliseconds:

~~~
YYYY-MM-DDTHH:MM:SS.mmmZ
~~~

Some resources also provide dates in a local time zone in addition to a regular timestamp. These properties typically end in `in_time_zone` or just `in_tz`.

~~~
2012-05-09T13:11:45.000+02:00
~~~

Authentication
--------------

The new Cabify API utilizes OAuth 2.0 authentication. For more details, see the [Authentication page](/authentication).


REST and Resources
------------------

The complete Cabify API follows strict REST concepts of resources. A resource is and end-point or URL path on which JSON documents can be created, retrieved, updated, and destroyed using HTTP commands.

The following table describes how the different HTTP actions correspond to activity on a resource end-point:

<table class="table">
  <thead>
    <tr>
      <th>GET</th>
      <th>POST</th>
      <th>PUT</th>
      <th>PATCH</th>
      <th>DELETE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Fetch a document</td>
      <td>Create a new document without an ID</td>
      <td>Replace or create a new document with a specific ID</td>
      <td>Update a subset of existing document attribtues</td>
      <td>Destroy the document completely</td>
    </tr>
  </tbody>
</table>

Each resource path may contain an ID parameter on the end. For example:

```
GET /api/journeys/09e1bc6081256de35996c69e24435f6f
```

Actions that fetch, modify or replace a resource such as GET, PUT, PATCH and DELETE typically require an id parameter to be present. Exceptions exist for resources that are unique to the user's connection and will be clearly defined in the documentation.

The Cabify API does not distinguish collections and single items, each are considered independent resources. The following examples are all independent resources:

```
GET /api/journey/09e1bc6081256de35996c69e24435f6f
GET /api/journeys
GET /api/regions
```

Error Handling
--------------

All error handling via the Cabify API is handled using HTTP status codes. Anything other than a `200 OK` response should be considered an error. There are several scenarios in which errors may occur:

1. Syntax and server errors will return either a `400 Bad Request` or `50X` reponses. Typically they will include a message body provided in text which should not be shown to the end user:

```
HTTP/1.1 400 Bad Request
Content-Length: 22

Problems parsing JSON.
```

2. If the server understands the request but no resource exists.

2. Requests that a syntantically valid or where the server is responding correctly but does not know how to deal with the request will attempt to return

```
HTTP/1.1 403 Forbidden

``` 
