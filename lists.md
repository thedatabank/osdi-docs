---
layout: default
title: "List"
---

# List

This document defines the List resource. 

Lists represent an arbitrary collection of objects. Lists can represent either arbitrary lists generated by users or the result of server processes such as query snapshots. For example, a list can represent people who are subscribed to receive a certain type of message. Lists have items which represent the individual resources that are part of the list's resulting collection. 

A list must fit a set of criteria:

* List results are arbitrary -- a resource's inclusion in the collection is based on having been put there by hand or by some kind of process and saved. For lists of resources that are created as a result of a computed process in real time, use the [Query](queries.html) resource instead. 
* Lists may only be static -- a list will return the resources it contains when the list was last modified or saved. To implement a dynamic list, which contains the resources which matched some criteria at this very moment, the [Query](queries.html) resource should be used instead.
* Lists are unique collections of resources -- a resource may match a list only once.


### Sections

* [Endpoints and URL structures](#endpoints-and-url-structures)
* [Fields](#fields)
    * [Common Fields](#common-fields)
    * [List Fields](#list-fields)  
    * [Links](#links)
* [Related Resources](#related-resources)
* [Scenarios](#scenarios)
    * [Scenario: Retrieving a collection of List resources (GET)](#scenario-retrieving-a-collection-of-list-resources-get)
    * [Scenario: Retrieving an individual List resource (GET)](#scenario-scenario-retrieving-an-individual-list-resource-get)
    * [Scenario: Creating a new list (POST)](#scenario-creating-a-new-list-post)
    * [Scenario: Modifying a list (PUT)](#scenario-modifying-a-list-put)
    * [Scenario: Deleting a list (DELETE)](#scenario-deleting-a-list-delete)


{% include endpoints_and_url_structures.md %}

The link relation label for a List resource is ```osdi:list``` for a single List resource or ```osdi:lists``` for a collection of List resources.

_[Back to top...](#)_


## Fields

{% include fields_intro.md %}

{% include global_fields.md %}

_[Back to top...](#)_


### List Fields

A list of fields specific to the List resource.

| Name          | Type      | Description
|-----------    |-----------|--------------
|origin_system  |string     |Human readable identifier of the system where this list was created
|name				|string		|The name of the list. Intended for administrative display rather than a public title, though may be shown to a user.
|title				|string		|The title of the list. Intended for public display rather than administrative purposes.
|description		|string		|A description of the list, usually displayed publicly. May contain text and/or HTML.
|summary			|string		|A text-only single paragraph summarizing the list. Shown on listing pages that have more than titles, but not enough room for full description.
|browser_url		|string		|A URL string pointing to the publicly available list page on the web.
|administrative_url		|string		|A URL string pointing to the list's administrative page on the web.
|total_items	|integer	|A read-only computed property representing the current count of the total number of items in the list.

_[Back to top...](#)_


### Links

{% include links_intro.md %}

| Name          | Type      | Description
|-----------    |-----------|-----------|--------------
|self			|[List*](lists.html)	|A self-referential link to the list.
|creator		|[Person*](people.html)  		|A link to a single Person resource representing the creator of the list.
|modified_by	|[Person* ](people.html) 		|A link to a Person resource representing the last editor of this list.
|items	|[Items[]*](items.html)	|A link to the collection of Item resources for this list.

_[Back to top...](#)_

## Related Resources

* [Item](items.html)
* [Person](people.html)

_[Back to top...](#)_

## Scenarios

{% include scenarios_intro.md %}

### Scenario: Retrieving a collection of List resources (GET)

List resources are sometimes presented as collections of lists. For example, calling the lists endpoint will return a collection of all the lists stored in the system's database associated with your api key.


#### Request

```javascript
GET https://osdi-sample-system.org/api/v1/lists/

Header:
OSDI-API-Token:[your api key here]
```

#### Response

```javascript
200 OK

Content-Type: application/hal+json
Cache-Control: max-age=0, private, must-revalidate

{
    "total_pages": 10,
    "per_page": 25,
    "page": 1,
    "total_records": 250,
    "_links": {
        "next": {
            "href": "https://osdi-sample-system.org/api/v1/lists?page=2"
        },
        "osdi:lists": [
            {
                "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3"
            },
            {
                "href": "https://osdi-sample-system.org/api/v1/lists/1efc3644-af25-4253-90b8-a0baf12dbd1e"
            },
            //(truncated for brevity)
        ],
        "curies": [
            {
                "name": "osdi",
                "href": "https://osdi-sample-system.org/docs/v1/{rel}",
                "templated": true
            }
        ],
        "self": {
            "href": "https://osdi-sample-system.org/api/v1/lists"
        }
    },
    "_embedded": {
        "osdi:lists": [
            {
                "identifiers": [
                    "osdi_sample_system:d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3",
                    "foreign_system:1"
                ],
                "origin_system": "OSDI Sample System",
                "created_date": "2014-03-20T21:04:31Z",
                "modified_date": "2014-03-20T21:04:31Z",
                "name": "Main Email List Subscribers",
                "summary": "The main email list.",
                "browser_url": "http://osdi-sample-system.org/lists/main-email-list",
                "administrative_url": "http://osdi-sample-system.org/lists/main-email-list/manage",
                "total_items": 1748920,
                "_links": {
                    "self": {
                        "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3"
                    },
                    "osdi:items": {
                        "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3/items"
                    },
                    "osdi:creator": {
                        "href": "https://osdi-sample-system.org/api/v1/people/65345d7d-cd24-466a-a698-4a7686ef684f"
                    },
                    "osdi:modified_by": {
                        "href": "https://osdi-sample-system.org/api/v1/people/c945d6fe-929e-11e3-a2e9-12313d316c29"
                    }
                }
            },
            {
                "identifiers": [
                    "osdi_sample_system:1efc3644-af25-4253-90b8-a0baf12dbd1e"
                ],
                "origin_system": "OSDI Sample System",
                "created_date": "2014-03-20T20:44:13Z",
                "modified_date": "2014-03-20T20:44:13Z",
                "title": "Membership List",
                "browser_url": "http://osdi-sample-system.org/lists/members",
                "administrative_url": "http://osdi-sample-system.org/lists/members/manage",
                "total_items": 108273,
                "_links": {
                    "self": {
                        "href": "https://osdi-sample-system.org/api/v1/lists/1efc3644-af25-4253-90b8-a0baf12dbd1e"
                    },
                    "osdi:items": {
                        "href": "https://osdi-sample-system.org/api/v1/lists/1efc3644-af25-4253-90b8-a0baf12dbd1e/items"
                    },
                    "osdi:creator": {
                        "href": "https://osdi-sample-system.org/api/v1/people/65345d7d-cd24-466a-a698-4a7686ef684f"
                    },
                    "osdi:modified_by": {
                        "href": "https://osdi-sample-system.org/api/v1/people/65345d7d-cd24-466a-a698-4a7686ef684f"
                    }
                }
            },
            //(truncated for brevity)
        ]
    }
}
```

_[Back to top...](#)_


### Scenario: Scenario: Retrieving an individual List resource (GET)

Calling an individual List resource will return the resource directly, along with all associated fields and appropriate links to additional information about the list.

#### Request

```javascript
GET https://osdi-sample-system.org/api/v1/lists/d32fcdd6-7366-466d-a3b8-7e0d87c3cd8b

Header:
OSDI-API-Token:[your api key here]
```

#### Response

```javascript
200 OK

Content-Type: application/hal+json
Cache-Control: max-age=0, private, must-revalidate

{
    "identifiers": [
        "osdi_sample_system:d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3",
        "foreign_system:1"
    ],
    "origin_system": "OSDI Sample System",
    "created_date": "2014-03-20T21:04:31Z",
    "modified_date": "2014-03-20T21:04:31Z",
    "name": "Main Email List Subscribers",
    "summary": "The main email list.",
    "browser_url": "http://osdi-sample-system.org/lists/main-email-list",
    "administrative_url": "http://osdi-sample-system.org/lists/main-email-list/manage",
    "total_items": 1748920,
    "_links": {
        "self": {
            "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3"
        },
        "osdi:items": {
            "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3/items"
        },
        "osdi:creator": {
            "href": "https://osdi-sample-system.org/api/v1/people/65345d7d-cd24-466a-a698-4a7686ef684f"
        },
        "osdi:modified_by": {
            "href": "https://osdi-sample-system.org/api/v1/people/c945d6fe-929e-11e3-a2e9-12313d316c29"
        }
    }          
}
```

_[Back to top...](#)_

### Scenario: Creating a new list (POST)

Posting to the list collection endpoint will allow you to create a new list. The response is the new list that was created. While each implementing system will require different fields, any optional fields not included in a post operation should not be set at all by the receiving system, or should be set to default values.


#### Request

```javascript
POST https://osdi-sample-system.org/api/v1/lists

Header:
OSDI-API-Token:[your api key here]

{
	"identifiers": [
        "foreign_system:1"
    ],
	"name": "Volunteers",
	"summary": "A list containing all of our volunteers",
	"origin_system": "OpenSupporter"
}
```

#### Response

```javascript
200 OK

Content-Type: application/hal+json
Cache-Control: max-age=0, private, must-revalidate

{
    "identifiers": [
        "osdi_sample_system:d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3",
        "foreign_system:1"
    ],
    "origin_system": "OSDI Sample System",
    "created_date": "2014-03-20T21:04:31Z",
    "modified_date": "2014-03-20T21:04:31Z",
    "name": "Volunteers",
    "summary": "A list containing all of our volunteers",
    "browser_url": "http://osdi-sample-system.org/lists/volunteers",
    "administrative_url": "http://osdi-sample-system.org/lists/volunteers/manage",
    "total_items": 0,
    "_links": {
        "self": {
            "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3"
        },
        "osdi:items": {
            "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3/items"
        },
        "osdi:creator": {
            "href": "https://osdi-sample-system.org/api/v1/people/65345d7d-cd24-466a-a698-4a7686ef684f"
        },
        "osdi:modified_by": {
            "href": "https://osdi-sample-system.org/api/v1/people/c945d6fe-929e-11e3-a2e9-12313d316c29"
        }
    }          
}
```

_[Back to top...](#)_

### Scenario: Modifying a list (PUT)

You can update a list by calling a PUT operation on that list's endpoint. Your PUT should contain fields that you want to update. Missing fields will be ignored by the receiving system. Systems may also ignore PUT values, depending on whether fields you are trying to modify are read-only or not. You may set an attribute to nil by including the attribute using `nil` for value.

{% include array_warning.md %}

#### Request

```javascript
PUT https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-de9uemdse

Header:
OSDI-API-Token:[your api key here]

{
    "name": "December Volunteers"
}

```

#### Response
```javascript
200 OK

Content-Type: application/hal+json
Cache-Control: max-age=0, private, must-revalidate

{
    "identifiers": [
        "osdi_sample_system:d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3",
        "foreign_system:1"
    ],
    "origin_system": "OSDI Sample System",
    "created_date": "2014-03-20T21:04:31Z",
    "modified_date": "2014-03-20T22:04:31Z",
    "name": "December Volunteers",
    "summary": "A list containing all of our volunteers",
    "browser_url": "http://osdi-sample-system.org/lists/volunteers",
    "administrative_url": "http://osdi-sample-system.org/lists/volunteers/manage",
    "total_items": 0,
    "_links": {
        "self": {
            "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3"
        },
        "osdi:items": {
            "href": "https://osdi-sample-system.org/api/v1/lists/d91b4b2e-ae0e-4cd3-9ed7-d0ec501b0bc3/items"
        },
        "osdi:creator": {
            "href": "https://osdi-sample-system.org/api/v1/people/65345d7d-cd24-466a-a698-4a7686ef684f"
        },
        "osdi:modified_by": {
            "href": "https://osdi-sample-system.org/api/v1/people/c945d6fe-929e-11e3-a2e9-12313d316c29"
        }
    }          
}
```

_[Back to top...](#)_


### Scenario: Deleting a list (DELETE)

You may delete a list by calling the DELETE command on the list's endpoint.

#### Request

```javascript
DELETE https://osdi-sample-system.org/api/v1/lists/d32fcdd6-7366-466d-a3b8-7e0d87c3cd8b

Header:
OSDI-API-Token:[your api key here]
```

#### Response

```javascript
200 OK

Content-Type: application/hal+json
Cache-Control: max-age=0, private, must-revalidate

{
    "notice": "This list was successfully deleted."
}
```

_[Back to top...](#)_
