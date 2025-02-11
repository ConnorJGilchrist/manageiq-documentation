---
---

## JSON Specification

### General Conventions

The API uses [JSON](http://www.json.org) throughout; the
[Content-Type](http://www.w3.org/Protocols/rfc1341/4_Content-Type.html)
for all requests and responses is **application/json**.

As is general practice with REST, clients should not make assumptions
about the server’s URL space. Clients are expected to discover all URL’s
by browsing the API. To keep this document readable, we still mention
specific URL’s, generally in the form of an absolute path. Clients
should not use these, or assume that the actual URL structure follows
these examples, and instead use discovered URL’s. Any client should
start its discovery with the API entry point, here denoted with
**/api**.

  - [Basic Types](#basic-types)

  - [Common Attributes and Actions](#common-attributes-and-actions)

      - [Attributes](#attributes)

  - [Actions](#actions)

  - [Collections](#collections)

  - [Action Specification](#action-specification)

  - [Collection Actions](#collection-actions)

  - [Resource Actions](#resource-actions)

  - [Action Responses](#action-responses)

### Basic types

The following are basic data types and type combinators that are used
throughout:

| Name           | Explanation                                                                                                       | Example serialization                                                                                                   |
| -------------- | ----------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Integer**    | Integer value                                                                                                     | { "memsize" : 2048 }                                                                                                    |
| **String**     | JSON string                                                                                                       | { "state" : "running" }                                                                                                 |
| **URL**        | Absolute URL                                                                                                      | { "href" : "http://SERVER/vms/1/start" }                                                                                |
| **Timestamp**  | Timestamp in ISO8601 format                                                                                       | { "created" : "2013-12-05T08:15:30Z" }                                                                                  |
| **Array\[T\]** | Array where each entry has type T                                                                                 | { "vms" : \[ { "id" : "1" }, { "id" : "2" }\] }                                                                         |
| **Ref\[T\]**   | A reference to a T, used to model relations, the T is a valid Resource identifier                                 | { "vm" : { "href" : URL } }                                                                                             |
| **Collection** | Array\[T\] where T represents a Ref\[T\], this might allow actions to be executed on all members as a single unit | { "vms" : { "count" : 2, "resources" : \[ { "href" : URL}, { "href" : URL } \], "actions" : \[\] }                      |
| **Struct**     | A structure with sub-attributes                                                                                   | "power\_state": {"state" : "ON", "last\_boot\_time" : "2013-05-29T15:28Z", "state\_change\_time" : "2013-05-29T15:28Z"} |

### Common Attributes and Actions

The following describes attributes and actions that are shared by all
resources and collections defined in this API.

#### Attributes

| Attribute | Type      | Description                                     |
| --------- | --------- | ----------------------------------------------- |
| id        | String    | A string identifier for the referenced resource |
| href      | Ref(self) | A unique self reference                         |
| name      | String    | A human name of the resource                    |

``` json
{
  "id" : "10",
  "href" : "http://localhost:3000/api/vms/10",
  "name" : "first_vm"
}
```

### Actions

| Action | HTTP method    | Description                           |
| ------ | -------------- | ------------------------------------- |
| create | POST           | Create new resource in the collection |
| edit   | PUT/PATCH/POST | Edit attributes in resource           |
| delete | DELETE         | Delete resource                       |

**Note:**

#### About permissions and security:

Advertising of the common actions depends purely on the role and permissions of that the current API user does have for the particular resource.

### Collections

Resources can be grouped into collections. Each collection is homogeneous so that it contains only one type of resource, and is unordered. Resources can also exist outside any collection. In this case, we refer to these resources as singleton resources. Collections are themselves resources as well.

Collections can exist globally, at the top level of an API, but can also be contained inside a single resource. In the latter case, we refer to these collections as sub-collections. Sub-collections are usually used
to express some kind of “contained in” relationship

Collections are serialized in JSON in the following way:

``` json
{
  "name" : String,
  "count": Integer,
  "subcount": Integer,
  "resources": [ ... ],
  "actions": [ ... ]
}
```

Where the **name** attribute is the collection name. The **count**
attribute in a collection always denotes the total number of items in
the collection, not the number of items returned. **subcount** attribute
in a collection depicts the number of items returned. Then the
**resources** attribute is an Array\[T\] where T might be a list of
references to the T or, if expanded a list of resources with all
attributes. The **actions** then contains an Array of actions that can
be performed against the collection resources.

### Action Specification

The representation of each resource will only contain an action and its
URL if the current user is presently allowed to perform that action
against that resource. Actions will be contained in the **actions**
attribute of a resource; that attribute contains an array of action
definition, where each action definition has a rel, method and a href
attribute.

  - **name** attribute contains the action name

  - **method** attribute states the HTTP method that must be used in a
    client HTTP request in order to perform the given action (eg. GET,
    POST, PUT, DELETE)

  - **href** attribute contains the absolute URL that the HTTP request
    should be performed against

  - **form** an optional attribute that references a JSON document which
    describes the resource attributes that can be provided in the
    message body when performing this action. This description will
    indicate which of those attributes are mandatory and which are
    optional.

### Collection actions

The actions performed against a collection of resources are in most
cases batch operations against multiple resources. The action request
must include an HTTP body with the action name and the list of resource
representations that the action will be performed against.

The resource representation might include the resource attributes as
they can change the way how the action is actually performed. In the
example below, the first service is retired immediately, versus the
second being retired at a later date with a retirement warning of 3
days.

Sample JSON request body for collection action:

    POST /api/services

``` json
{
  "action": "request_retire",
  "resources" : [
    { "href" : "http://localhost:3000/api/services/101" },
    { "href" : "http://localhost:3000/api/services/102",
      "date" : "10/30/2015",
      "warn" : 3
    }
  ]
}
```

Actions in collection:

``` json
{
  "name" : String,
  "count": Integer,
  "resources": [ ... ],
  "actions": [
    {
      "name"   : "start",
      "method" : "post",
      "href"   : URL
    },
    {
      "name"   : "stop",
      "method" : "post",
      "href"   : URL
    },
    {
      "name"   : "suspend",
      "method" : "post",
      "href"   : URL
    },
    {
      "name"    : "edit",
      "method" : "post",
      "href"   : URL
    },
    {
      "name"    : "delete",
      "method" : "post",
      "href"   : URL
    },
    {
      "name"   : "delete",
      "method" : "delete",
      "href"   : URL
    }
  ]
}
```

### Resource actions

An action performed against a given resource is always described in the
body of the HTTP request. The HTTP body could contain a list of resource
attributes that dictate how the state of the receiving resource is to be
changed once the action is performed. At minimum the JSON document in
the message body must contain the name of the action to be performed.

In cases where no attributes are required to perform an action the HTTP
body will contain an empty JSON document, in which case default values
will be assigned to the corresponding attributes.

Sample JSON request body for resource action:

    POST /api/services/101

``` json
{
  "action"   : "request_retire",
  "resource" : { "date" : "10/30/2015", "warn" : 5 }
}
```

    POST /api/vms/321

``` json
{
  "action"   : "start",
  "resource" : {}
}
```

or Simply:

``` json
{
  "action"   : "start"
}
```

Actions in a resource:

``` json
{
  "id"    : String,
  "href"  : Ref(self),
  "name"  : "resource human name",
  "actions" : [
    {
      "name"   : "edit",
      "method" : "post",
      "href"   : URL
    }
  ]
}
```

### Action Responses

When performing actions on resources, there are two types of responses
that one is to expect.

1.  For actions that operate on the resource itself like a *create* or
    *edit*, the response is usually the updated resource. This includes
    creation of Provision or Automate requests where the created
    /api/provision\_requests and /api/automation\_requests gets
    returned.

2.  For others like a *start* or *stop* action, the response includes an
    action result for each targetted resource. An action result has the
    following construct in the response:

<!-- end list -->

``` json
{
  "results" : [
    {
      "success" : true | false,
      "message" : String,
      "href" : Ref[resource],
      "result" : Struct,
      "task_id" : Id,
      "task_href" : Ref[task]
    },
    ...
  ]
}
```

results being an array of action results as one or more resources could
be targeted in a request.

NOTE:

  - success and message are always there.

  - result is optional and would exist when an action results in data,
    i.e. policy resolve.

  - href is populated for the resource being targeted by the action

  - task\_id and task\_href are optional. They are defined when actions
    are run asynchronously and a task is created, i.e.
    <http://localhost:3000/api/tasks/:id> which can be monitored for
    action completion.

Other action specific attributes could also be returned in the the
action result.

For tagging actions:

``` json
{
  "tag_category" : String,
  "tag_name" : String,
  "tag_href" : Ref[tags]
}
```

When executing actions on subcollections, like *policies*,
*policy\_profiles* and *service\_templates*, the following is also
provided in the action result:

``` json
{
  "<subcollection>_id" : Id,
  "<subcollection>_href" : Ref[subcollection]
}
```
