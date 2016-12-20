---
title: Sense for Organisers API Reference

language_tabs:
  - python
  - javascript
  - php


toc_footers:
  - <a href='#'>lol</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

**This is the API Docs for Sense for Organisers.**

###Click [Here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) if you need the reference to the Markdown Syntax.

## Request and Response format

### Requests

All requests to the API should be encoded as JSON, with the `content-type` header set to `application/json`. The correct HTTP verb should also be observed. In general:

* `GET`: Obtain index or one or more records
* `POST`: Creating one or more new records
* `DELETE`: Delete one or more records

### **Deletion:**

All deletion on the API are hard deletes. There is no recovery or undo from the API side.

### **Responses:**

All API responses are also be encoded as JSON. A 2xx status code is returned in the case of successful requests.

All successful POST requests creating a new resource will return a Location header representing the URL of the newly created resource, and all `POST` requests will return the new state of the object in the body. 

Most objects also come with an url field that indicates the API endpoint that represents the object.

### **Content Safety:**

Unless otherwise specified, all strings returned by the API should be considered unsafe and should be XSS filtered by the consumer.

### **Errors:**

Response with 4xx or 5xx status codes indicate something went wrong with the request. 4xx errors indicate a problem with the request, and will carry a debugging message for the developer. Except for validation error in the case of 400 errors (see below), this should not be displayed to the end user.

*The error codes and causes can be found at the bottom of this document*

# /analytics/acquisition

## /event_information/{event_id}

> The command returns JSON structured like this:

```json
{
 "groups": [
  {
    "group_id": "all",
    "group_name": "All"
  },
  {
    "group_id": 1,
    "group_name": "sellers"
  },
  {
    "group_id": 2,
    "group_name": "buyers"
  }
],
"attributes": [
  {
    "attribute_id": "people",
      "attribute_name": "People"
  },
  {
    "attribute_id": "company",
    "attribute_name": "Company"
  },
  {
    "attribute_id": "1",
    "attribute_name": "Title"
  },
  {
    "attribute_id": 2,
    "attribute_name": "Location"
  }
 ]
}
```

<aside class="success"><b>GET</b></aside>

Returns avaliable attributes and participant groups from a specific event.

## /handshake_breakdown/{event_id}/  {category}/{attendee_group}/{received}

> The command returns JSON structured like this:

```json
{
  "data": [
    { //for people
      "category_data": "29127",
      "category_name": "Mr Lit Cheong Chong",
      "category_sub": "Capitaland Limited",
      "handshake_SorR": "3",
      "person_type": "0",
      "respond_rate": "0.0%",
      "made_action": 0
    },
    { //for company
      "category_data": "Capitaland Limited",
      "category_name": "Capitaland Limited",
      "category_sub": "",
      "handshake_SorRac": "2",
      "person_type": "",
      "respond_rate": "0.0%"
    },
    { //for industry & products & position
      "category_data": "Real Estate",
      "category_name": "Real Estate",
      "category_sub": "",
      "handshake_SorR": "2",
      "person_type": "0",
      "respond_rate": "0.0%"
    },
    { //for country
      "category_data": "Singapore",
      "category_name": "Singapore",
      "category_sub": "",
      "handshake_SorR": "2",
      "person_type": "0",
      "respond_rate": "0.0%"
    }
 ] 
}
```

<aside class="success"><b>GET</b></aside>

Returns the handshake breakdown for a specific event, consisting of the category, the person type and the whether request have been received.

Category | Description
--------- | -----------
1 | People
2 | Company
3 | Title
4 | Product
5 | Industry
6 | Country

Parameter | Description
--------- | -----------
event_id | ID of the event
category | same category as above
first_sorter | first sorting category
second_sorter | second sorting category

First Sorter | Description
--------- | -----------
0 | Buyer
1 | Exhibitor
2 | Both

<aside class="notice">
always pass either 0 for speednetworking
</aside>

Second Sorter | Description
--------- | -----------
0 | Sent Handshake
1 | Received Handshake

## /handshake_info/{event_id}/{category}/  {category_string}/{first_sorter}/  {second_sorter}


> The command returns JSON structured like this: 

```json
{
  "data": [
  {
    "category_name": "some_name", 
    "category_sub": "some_company", 
    "person_type":"1",
    "handshake_status": "1",
    "success_rate": "20.1",
    "archived": 0,
    "bookmarked": 0,
    "viewed": 0,
    "made_action": 0
  },
  {
    "category_name": "some_name2",
    "category_sub": "some_company2",
    "person_type": "1",
    "handshake_status": "0",
    "success_rate": "20.1"
  }
 ] 
}
```

<aside class="success"><b>GET</b></aside>

Returns handshake information from a particular event id, containing the category, first sorter and second sorter

Parameter | Description
--------- | -----------
event_id | ID of the event
category | same category as above
first_sorter | first sorting category
second_sorter | second sorting category

First Sorter | Description
--------- | -----------
0 | Buyer
1 | Exhibitor
2 | Both

<aside class="notice">always pass either 0 for speednetworking</aside>

Second Sorter | Description
--------- | -----------
0 | Sent Handshake
1 | Received Handshake

### **Handshake_status:**

Handshake | Status
--------- | -----------
-2 | Successful (tick)
-1 | No response
0 | "There is no specific reason"
1 | "My event schedule is full"
2 | "There is a lack of interest at this time"
3 | "Your products/projects/services may not fit our requirements"
4 | "You may not fit our investment criteria at this moment"

## /inner_handshake_info/{event_id}/  {category/{upper category}/  {lower_category}/{first_sorter}/  {second_sorter}

> The command returns JSON structured like this: 

```json
{
  "data": [  
    {
    "category_name": "some_name", 
    "category_sub": "some_company", 
    "person_type":"1",
    "handshake_status": "1",
    "made_action": "0" },
    {
    "category_name": "some_name2",
    "category_sub": "some_company2",
    "person_type":"1",
    "handshake_status": "0",
    "made_action":"0" }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns handshake information from a particular event id, containing the UPPER and LOWER category, first sorter and second sorter

Parameter | Description
--------- | -----------
event_id | ID of the event
category | same category as above
first_sorter | first sorting category
second_sorter | second sorting category
upper_category | the 1st layer string
lower_category | the 2nd layer string

<aside class="notice">
<i>
<b>example</b>
<p>Oil --> Pharma --> People</p>  Upper category will be Oil
<p>Lower category will be Pharma</p> 
</i>
</aside>

First Sorter | Description
--------- | -----------
0 | Buyer
1 | Exhibitor
2 | Both

<aside class="notice">always pass either 0 for speednetworking</aside>

Second Sorter | Description
--------- | -----------
0 | Sent Handshake
1 | Received Handshake

## /chart_data/{eventid}/{group_id}
 
> The command returns a JSON structured like this

```json
 {	
  "group_size": "40",  // len of group
  "request_percentage": "30.1",
  "total_requests": "133" 
 } // total hs sent all 

{	"data": [
{	  "nodes": [
	    "unsuccessful",
	    "archived" ],
	  "size": 0 },
{	  "nodes": [
	   	"unsuccessful",
		  "starred"  ],
	  "size": 0 },
{	  "nodes": [
		  "Unsuccessful Requests",
		  "Declined" ],
	  "size": 0 },
{	  "nodes": [
		  "unsuccessful",
		  "nostatus" ],
	  "size": 3936 },
{	  "nodes": [
		  "successful",
		  "archived" ],
	  "size": 29 },
{	  "nodes": [
	   	"successful",
	   	"starred" ],
    "size": 117 },
{	  "nodes": [
		 "successful",
     "nostatus" ],
    "size": 1608
    }
  ]
 }
```

<aside class="success"><b>GET</b></aside>


Returns chart data consisting of total requests and request percentage for the given event id

## /line_chart_data/{event_id}/{group_id}

>Same as engagement chart

# 2. /analytics/meeting

## /event_information/{int:event_id}

```json
{
  "groups": [
   {
      "group_id": "all_attendee",
      "group_name": "All Attendee",
      "is_exhibitor": 0
    },
    {
      "group_id": "all_booth",
      "group_name": "All booth",
      "is_exhibitor": 1
    },
    {
      "group_id": 2,
      "group_name": "buyers",
      "is_exhibitor": 1
    }
  ]
 }
```

<aside class="success"><b>GET</b></aside>

Returns the event information consisting of the group id, the group and exhibitor type of a partiular event

### **default groups include:**

* all_attendee
* all_booth
* timeslot
* table
* pending

## /table/{int:event_id}/{int:category}

```json
{
  "data": [
    {
      "header_list": [
    	{
      	"data_list": [
        	{
          	"data_1": "Michael Espiritu",
          	"data_2": "Kang Nai Jin",
                "person_type_1":  "1",
                "person_type_2": "0",
          	"side_data": "",
          	"sub_data_1": "Crearis Environmental Design",
          	"sub_data_2": "Longyou Greentimber Technology Bamboo & Wood Co.,Ltd"
        	},
        	{
          	"data_1": "Steven Ng",
          	"data_2": "Kang Nai Jin",
          	"side_data": "",
          	"sub_data_1": "Nature Landscapes Pte Ltd",
          	"sub_data_2": "Longyou Greentimber Technology Bamboo & Wood Co.,Ltd"
        	}
      	],
      	"header": ""
    	}
  	],
  	"sub_title": "Pending",
  	"title": ""
	},
    {
  	"header_list": [],
  	"sub_title": "Manitou Asia Pte Ltd",
  	"title": "A01"
	},
	{
  	"header_list": [
    	{
      	"data_list": [
        	{
          	"data_1": "Michael Espiritu",
          	"data_2": "Kang Nai Jin",
          	"side_data": "15:30",
          	"sub_data_1": "Crearis Environmental Design",
          	"sub_data_2": "Longyou Greentimber Technology Bamboo & Wood Co.,Ltd"
        	},
        	{
          	"data_1": "Steven Ng",
          	"data_2": "Kang Nai Jin",
          	"side_data": "18:00",
          	"sub_data_1": "Nature Landscapes Pte Ltd",
          	"sub_data_2": "Longyou Greentimber Technology Bamboo & Wood Co.,Ltd"
        	}
      	],
      	"header": "07-Nov-13"
    	}
  	],
  	"sub_title": "Longyou Greentimber Technology Bamboo & Wood Co.Ltd",
  	"title": "A05"
	},
	{
  	"header_list": [],
  	"sub_title": "5 Color Time and Space Eco-Engineering Pte Ltd",
  	"title": "A10"
	}
]
}
```

<aside class="success"><b>GET</b></aside>

Returns a list of meetings consisting of both the participants and the their company names for an event

 Category | Description
--------- | -----------
3 | Timeslot
4 | Table

<aside class="notice">
<b>JSON example is for timeslots and table</b>
</aside>

## /table/{int:event_id}/{int:category}

```json
// for exhibitor
{
	"data": {
	  "exhibitor_id": "x",
	  "booth_no": "x",
	  "company_name": "Jublia",
	  "average_rating": 3,
	  "rating_quantity": 8,
	  "representative": 
		{
		"rep_name": "Yan",
		"rep_id": "xxxxx",
		"meeting_quantity" : "x",
		"rep_avg_rating": 3,
		"rep_rating_qty": 3,
		"no_show_rate": 30.0
		}
	}
}

// for buyer
{
	"data": {
	  "attendee_id": "x",
	  "attendee_name": "Chinab",
	  "company_name": "Jublia",
	  "average_rating": 3,
	  "rating_quantity": 8,
	  "meeting_quantity": 5,
	  "no_show_rate": 23.3,
	  "meetings": {
	  "meeting_date": "xx-xx-xxxx",
	  "meetings": 
		{
		  "meeting_time": "xx:xx",
		  "rated_by": "xx_name",
		  "company_name": "xx_company",
		  "rating_given": 3,
		  "feedback": "if this field is empty, trim and pass empty string"
		}
	}
 }
}
```

<aside class="success"><b>GET</b></aside>

Returns a list of meetings with  data for a specific event and category

 Category | Description
--------- | -----------
1 | Attendee
2 | Exhibitor (prev. Booth/ Table)

## /meeting_info/{int:event_id}/  {id_attendee_login}

```json
{
  "data": {
	"meeting_date": "xx-xx-xxxx",
	"meetings": 
    {
      "meeting_time": "xx:xx",
      "rated_by": "xx_name",
      "company_name": "xx_company",
      "rating_given": 3,
      "feedback": "if this field is empty, trim and pass empty string"
    }
 }
}
```

<aside class="success"><b>GET</b></aside>

Returns the meeting infomation of an attendee for an event

<aside class="warning">
<b>
AS OF NOW, THIS IS THE STANDARD INNER MEETING FOR EXHIBITOR AND BUYER CATEGORY
<p>TIMESLOT AND TABLES ARE NOT</p>
</b>
</aside>

## /rating_stats/{int:event_id}

```json
{
  "dlg_data": 
    {
      "color": "rgba(0,0,0,0.7)",
      "key": "78 ratings, 20 people rated",
      "values": [
        {
          "label": "5 stars",
          "value": 33
        },
        {
          "label": "4 stars",
          "value": 26
        },
        {
          "label": "3 stars",
          "value": 15
        },
        {
          "label": "2 stars",
          "value": 3
        },
        {
          "label": "1 star",
          "value": 1
        },
        {
          "label": "No show",
          "value": 8
        }
      ]
    }
}
```

<aside class="success"><b>GET</b></aside>

Returns the rating statistics of the event

<aside class="notice">
<b>exb_data would be {} or empty if th event does not have exhibitors</b>
<p><i>all returned numbers are integers</i></p>
</aside>

## /pending_meeting/{int:event_id}

```json
{
  "pending_count": 0,
  "data":
  [
	{
    "data_1": "Michael Espiritu",
    "data_2": "Kang Nai Jin",
	"person_type_1": "1",
	"person_type_2": "0",
	"sub_data_1": "Crearis Environmental Design",
	"sub_data_2": "Longyou Greentimber Technology Bamboo & Wood Co.,Ltd"
	},
	{
	"data_1": "Steven Ng",
	"data_2": "Kang Nai Jin",
	"sub_data_1": "Nature Landscapes Pte Ltd",
	"sub_data_2": "Longyou Greentimber Technology Bamboo & Wood Co.,Ltd"
	}
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the pending meetings of all participants in a given event

<aside class="notice">
Pending is now isolated. This is the new one. Although it looks the same as the old one.
</aside>

## /chart_data/{eventid}

```json
{
  "chart": 
[
	{
	"date": "DD/MMM/YY",
    "day": "1",
  	"type": "DD-MMM-YY HH:MM",
  	"Meetings": "40"
	},
	{
   	"date": "DD/MMM/YY",
    "day": "2",
  	"type": "DD-MMM-YY HH:MM",
  	"Meetings": "40"
	}
],
  "total_timeslots": "52",
  "avg_meetings_per_timeslot": "1.9",
  "total_meetings_count": "98"
}
```

<aside class="success"><b>GET</b></aside>

Returns chart data for the given event id

## /total_meetings/{eventid}

```json
{
  "total_meetings_count": "98",
  "total_timeslots": "52"
}
```

<aside class="success"><b>GET</b></aside>

Returns the grand total of meetings of the event

## /avg_meetings_per_timeslot/{eventid}

```json
{
  "avg_meetings_per_timeslot": "1.9",
  "total_timeslots" : "52"
}
```

<aside class="success"><b>GET</b></aside>

Returns the average of meetings per timeslot of the event

## /meeting_stats/{eventid}/{page}

```json
{
  "attendee_meeting_count": "xx", // total meetings made the group
  "attendee_count": "xx", // total attendee of the group
  "is_exhibitor": "xx",
  "attendee_has_meeting": "xx", // attendee of the group that has meeting,
  "booth_has_meeting": "xx", // only if ies_exhibitor = 1; booth that has meeting
  "booth_count": "xx"
}
```

<aside class="success"><b>GET</b></aside>

Returns the meeting stats of an event consisting of the number of participants and meetings made 

## /download_csv/{int:event_id}

<aside class="success"><b>GET</b></aside>

This downloads the CSV for a given event

# 3. /analytics/trends

## /event_information/{int:event_id}

```json
{
  "attributes": [
    {
      "attribute_id": "people",
      "attribute_name": "People"
    },
    {
      "attribute_id": "company",
      "attribute_name": "Company"
    },
    {
      "attribute_id": "1",
      "attribute_name": "Title"
    },
    {
      "attribute_id": "2",
      "attribute_name": "Location"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the event information for a given event 

## /trends_details/{str:type}/{str:tool}/  {int:event_id}

```json
{
  "trends": [
   {
      "demographics": "Products",
      "demographics_id": 1,
      "top_10": [
        {
          "name": "Skateboard",
          "search_count": "10"
        },
        {
          "name": "Moonboard",
          "count": "9"
        },
        ]
    },
   {
      "demographic": "Industry",
	  "demographics_id": 2,
      "top_10": [
        {
          "name": "Medical",
          "count": "10"
        },
        {
          "name": "Maritime",
          "count": "9"
        }
      ]
	}
  ] 
 }
```

<aside class="success"><b>GET</b></aside>

Returns the trend details for a given search or interest, in match open or match, for an event

**type:** 

* search 
* interests

**tool:** 

* match_open
* match

## /more_trends/{str:type}/{str:tool}/  {int:event_id}/{demographic_id}

```json
{
  "trends": 
  {
     "demographics": "Industry",
     "demographics_id": 1,
      "full_list": [
        {
        "name":"Skateboard",
        "search_count": "10"
        },
        {
        "name": "Moonboard",
        "count": "9"
        }
     ]
  } 
}
```

<aside class="success"><b>GET</b></aside>

**type:** 

* search
* interests

**tool:** 

* match_open 
* match

## inner_trends?trend_type=' '&tool_type=' '&event_id=' '&demographics_id=' '&trend_term=' '

```json
{
   "related_trends": 
         [
            {
              "name": "hotdog",
              "count": 3
            },
            {
              "name": "sausage",
              "count": 2
            }
          ]
 }
```
 
<aside class="success"><b>GET</b></aside>

## demographics/{trend_type}/{tool_type}/ {int:event_id}/{int:requested_attribute_id}/ {int:demographics_id}/{trend_term}

```json
{
 "demographics": [
        {
          "name": "Buyers / Visitors", 
          "count": 34
        },
        {
          "name": "Exhibitor",
          "count": 4
        }
      ],
 "attributes": [
        {
          "name": "CEO", 
          "count": 4
        },
        {
          "name": "CTO",
          "count": 3
        }
      ]
}
```

<aside class="success"><b>GET</b></aside>

# 4. /engagement/vistsviews

## /event_information/{int:event_id}
```json
{
  "groups": [
   {
      "group_id": "all",
      "group_name": "All"
    },
    {
      "group_id": 1,
      "group_name": "sellers"
    },
    {
      "group_id": 2,
      "group_name": "buyers"
    }
  ],
  "attributes": [
    {
      "attribute_id": "people",
      "attribute_name": "People"
    },
    {
      "attribute_id": "company",
      "attribute_name": "Company"
    },
    {
      "attribute_id": 1,
      "attribute_name": "Title"
    },
    {
      "attribute_id": 2,
      "attribute_name": "Location"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the event information for a given event consisting of the group and attributes

## /vv_breakdown/{event_id}/{category}/ {first_sorter}/{second_sorter}

```json 
{
  "data": [
  { // FOR PEOPLE
    "category_data": "23080",
    "category_name": "Tan Kuan Yan",
    "category_sub": "Jublia Pte Lt",
    "vv_count": "xx"
  },
  {  // FOR COMPANY, POSITION, TITLE, COUNTRY, PRODUCT
    "category_data": "Jublia Pte Ltd",
    "category_name": "",
    "category_sub": "",
    "vv_count": "xx"
  }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns visit and views breakdown sorted according to the string field

Category | Description
--------- | -----------
1 | People
2 | Company
3 | Title
4 | Product
5 | Industry
6 | Country

Parameter | Description
--------- | -----------
event_id | ID of the event
category | same category as above
first_sorter | first sorting category
second_sorter | second sorting category

First Sorter | Description
--------- | -----------
0 | Buyer
1 | Exhibitor
2 | Both

<aside class="notice">always pass either 0 for speednetworking</aside>

Second Sorter | Description
--------- | -----------
0 | View
1 | Visit / login


## /vv_info/{event_id}/{category}/ {category_string}/{first_sorter}/ {second_sorter}

```json
{
   "data": [ 
  {
    "category_name": "some_name", 
    "category_sub": "", 
    "vv_count": "1",
    "person_type": "1" 
  },
  {
    "category_name": "some_name", 
    "category_sub": "", 
    "vv_count": "1",
    "person_type": "1"
  }
  ]
} 
```

<aside class="success"><b>GET</b></aside>

Returns info of the visit and views breakdown. It should return the name and count

Category | Description
--------- | -----------
1 | People
2 | Company
3 | Title
4 | Product
5 | Industry
6 | Country

Parameter | Description
--------- | -----------
event_id | ID of the event
category | same category as above
first_sorter | first sorting category
second_sorter | second sorting category

First Sorter | Description
--------- | -----------
0 | Buyer
1 | Exhibitor
2 | Both

<aside class="notice">always pass either 0 for speednetworking</aside>

Second Sorter | Description
--------- | -----------
0 | View
1 | Visit / login

## /chart_data/{eventid}

```json
{
  "data": [
  {
    "key": "Views",
    "color": "somehexcolorvalue",
    "values": [{"x": 1028088000, "y": 10},{"x": 1030766400, "y": 10}]
  },
  {
    "key": "Visits",
    "color": "somehexcolorvalue",
    "values": [{"x": 1028088000, "y": 10},{"x": 1030766400, "y": 10}]
  }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the a JSON data containing daily views and visitors

## /vv_stats/{eventid}/{person_group}

```json
{
    "average_login_count": "xx",
    "total_login": "xx",
    "average_views_per_day" : "10",
    "total_views": "xx"
}
```

Returns visits and views stats

# 5. /engagement/feedback

## /event_information/{int:event_id}

```json
{
  "groups": [
   {
      "group_id": "all",
      "group_name": "All"
    },
    {
      "group_id": "timeline",
      "group_name": "Timeline"
    }
    {
      "group_id": 1,
      "group_name": "sellers"
    },
    {
      "group_id": 2,
      "group_name": "buyers"
    }
  ]
 }
```

<aside class="success"><b>GET</b></aside>

Returns event information for a given event

<aside class="notice">
<b>JSON is for TIMESLOT</b>
</aside>

## /display/{int:event_id}/ {int:person_category}

```json
{
  "data": [
    {
      "date": "20-12-1014",
      "feedbacks": [
        {
          "person_name": "Andriano",
          "person_company": "Jublia",
          "person_type": 0,
          "feedback": "Not enough Pizza",
          "timestamp": "17:20",
          "sentiments": "Positive"
        },
        {
          "person_name": "Andriano",
          "person_company": "Jublia",
          "feedback": "Not enough Pizza",
          "timestamp": "17:20"
        },
        {
          "person_name": "Andriano",
          "person_company": "Jublia",
          "feedback": "Not enough Pizza",
          "timestamp": "17:20"
        },
        {
          "person_name": "Andriano",
          "person_company": "Jublia",
          "feedback": "Not enough Pizza",
          "timestamp": "17:20"
        }
      ]
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns feedback information of a participant for a particular event

<aside class="notice">
<b>JSON is for TIMESLOT</b>
</aside>

## /display/{int:event_id}/ {int:person_category}

```json
{
  "data": [
    {
      "person_name": "Andriano",
      "person_company": "Jublia",
      "feedbacks": [
        {
          "feedback": "Not enough pizza",
          "timestamp": "12-21-1992 12:00"
        }
      ]
    },
    {
      "person_name": "Andriano",
      "person_company": "Jublia",
      "feedbacks": [
        {
          "feedback": "Not enough pizza",
          "timestamp": "12-21-1992 12:00"
        }
      ]
    },
    {
      "person_name": "Andriano",
      "person_company": "Jublia",
      "feedbacks": [
        {
          "feedback": "Not enough pizza",
          "timestamp": "12-21-1992 12:00"
        }
      ]
    },
    {
      "person_name": "Andriano",
      "person_company": "Jublia",
      "feedbacks": [
        {
          "feedback": "Not enough pizza",
          "timestamp": "12-21-1992 12:00"
        }
      ]
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns feedback information of a participant for a particular event

<aside class="notice">
<b>JSON is for PERSON</b>
</aside>

## /data_input

> TDB

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
event_id | ID of the event; 60
feedback_param | ["person id", “date”, "time", "feedback"]

## /autocomplete/event_id/{word}

```json
{
  "data": [
    {
      "name": "Andriano",
      "email": "andriano@Jublia.com",
      "id_attendee_login": 12345
    },
    {
      "person_name": "Chinab",
      "company_name": "chinab@Jublia.com",
      "person_id": 12346
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Autocomplete the event name

## /communication/feedback/stats/{event_id}

```json
{
  "exb_feedback": 20,
  "dlg_feedback": 30,
  "total_feedback": 60
}
```

<aside class="success"><b>GET</b></aside>

Returns the feedback stats by exhibitor, delegete and the total

<aside class="notice">
Feedback counts are unique for exb and dlg.

<p>Total feedbacks are not</p>

<p><i>e.g one exb making two feedbacks will be counted as 1 exb_feedback and 2 total feedback</i></p>
</aside>

## /communication/feedback/stats/ {int:category}

> TBD

<aside class="success"><b>GET</b></aside>

## /message_breakdown/{int:eventid}

```json
{
  "message_list": [
  {
    "question_id": "1", // for query
    "timestamp": "DD MM YY - HH:mm",
    "feedback_message": "Hi, how did your networking session go? /Jublia",
    "sent_amount": "30",
    "unsucessful_amount": "15",
    "sent_total": "45",
    "reply_amount": "9",
    "sent_rate": "50",
    "respond_rate": "25",
    "replies": [
      {
        "company_name": "Some_green_company",
        "message": "Green trees are green, Thanks",
        "name": "Marcus ",
        "sender": "+6593719500",
        "timestamp": "DD MM YY - HH:mm"
      }
    ]
  }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the feedback message and replies over the course of the event

## /wordcloud/{int:eventid}/{int:group}

```json
{
   "children": [
    {
     "name": "good",
     "children": [
      {"name": "AgglomerativeCluster", "size": 3},
      {"name": "CommunityStructure", "size": 2},
      {"name": "HierarchicalCluster", "size": 1},
      {"name": "MergeEdge", "size": 5},
      {"name": "AgglomerativeCluster", "size": 3},
      {"name": "CommunityStructure", "size": 2},
      {"name": "HierarchicalCluster", "size": 1},
      {"name": "MergeEdge", "size": 5}
     ]
    },
    {
     "name": "bad",
     "children": [
      {"name": "AgglomerativeCluster", "size": 3},
      {"name": "CommunityStructure", "size": 2},
      {"name": "HierarchicalCluster", "size": 1},
      {"name": "MergeEdge", "size": 5},
      {"name": "AgglomerativeCluster", "size": 3},
      {"name": "CommunityStructure", "size": 2},
      {"name": "HierarchicalCluster", "size": 1},
      {"name": "MergeEdge", "size": 5}
     ]
   }
   ]
 }
```

<aside class="success"><b>GET</b></aside>

Generate a string from the list of feedback message received.
The string would be stripped bare of punctuation, numbers and stop words


Group | Description
--------- | -----------
1 | Delegates
2 | Exhibitors

# 6. /tools/communication

## /test

> No return

<aside class="success"><b>POST</b></aside>

Test with engagement manager

Parameter | Description
--------- | -----------
event_id | 23
email_body |  "Hi [name]<br></br> Thanks!""
subject | "[event_name]: Networking Opportunity"
cat 1 | "all" or “new” or "ext"
cat 2 | "all" or participant_groups
cat 3 | "pending" or "not_pending" or "logged_in" or "not_logged_in" or "sent" or "not_sent" or "na" or "pending_meetings" or "meetings" or "rated" or "not_rated"
cat 4 | cat4 = "email" or "sms"

## /details

> No return

<aside class="success"><b>POST</b></aside>

Called immediately after ‘send to all’ is pressed to verify details of the email being sent

Parameter | Description
--------- | -----------
event_id | 23
email_body |  "Hi [name]<br></br> Thanks!""
subject | "[event_name]: Networking Opportunity"
cat 1 | "all" or “new” 
cat 2 | "all" or participant_groups
cat 3 | "pending" or "not_pending" or "logged_in" or "not_logged_in" or "sent" or "not_sent" or "na" or "pending_meetings" or "meetings" or "rated" or "not_rated"
cat 4 | cat4 = "email" or "sms"

## /details_log

```json
{
"event": "start_processing",
"data": {
  "event_name": "GUSA 2013",
  "subject": "GUSA 2013: Call for networking"
},
"event": "communication_details",
"data": {
  "no_of_comms": 123, 
  "comms_content": "Hi Andriano<br/><br/>Thanks!"
  },
"event": "close",
"data": {
  "close": "close stream"
},
"event": "error",
"data": {
  "error": "details logger crashed"
 }
}
```

<aside class="success"><b>GET (SSE)</b></aside>

Retrieves details from /details

**STORED IN A SESSION**

Parameter | Description
--------- | -----------
event_id | 23
email_body |  "Hi [name]<br></br> Thanks!""
subject | "[event_name]: Networking Opportunity"
cat 1 | "all" or “new” 
cat 2 | "all" or participant_groups
cat 3 | "pending" or "not_pending" or "logged_in" or "not_logged_in" or "sent" or "not_sent" or "na" or "pending_meetings" or "meetings" or "rated" or "not_rated"
cat 4 | cat4 = "email" or "sms"

## /all

```json
{
"event": "starting_stream",
"data": {
  "process": "email sender"
},
"event": "start_sending",
"data": {
  "total_email": 100
},
// on loop
"event": "email_sent",
"data": {
  "percentage_sent": 35.55
},
"event": "close",
"data": {
  "close" : "close stream"
},
"event": "error",
"data": {
  "error": "someone sending emails"
},
"event": "error",
"data": {
  "error": "email sender crashed"
 }
}
```

<aside class="success"><b>GET (SSE)</b></aside>

When the /details is confirmed as yes, this API is called to blast out the email to everyone

**STORED IN A SESSION**

Parameter | Description
--------- | -----------
event_id | 23
email_body |  "Hi [name]<br></br> Thanks!""
subject | "[event_name]: Networking Opportunity"
cat 1 | "all" or “new” or "ext"
cat 2 | "all" or participant_groups
cat 3 | "pending" or "not_pending" or "logged_in" or "not_logged_in" or "sent" or "not_sent" or "na" or "pending_meetings" or "meetings" or "rated" or "not_rated"
cat 4 | "email" or "sms"

## /single_text_emailsender

> No return

<aside class="success"><b>POST</b></aside>

Used for sending a real-time rate email after the last meeting of the person. Called from reminder_system.py

Parameter | Description
--------- | -----------
event_id | 100
person_id | 101356
email_type | "rate_real_time"

## /log/{int:event_id}

```json
{
"log": 
 [ 
	{
	 "sender": "Andriano Winatra",
     "emails_sent": 200,
	 "emails_opened": 150,
	 "cat1": "all",
	 "cat2": "del",
	 "cat3": "pending",
	 "message": "<p class='line' idtartingUP 2013)</p>",
	 "subject": "hello",
	 "timestamp": "2014-05-26 12:34:11"
    },
    {  
	"cat1": "all",
	"cat2": "del",
	"cat3": "pending",
	"message": "<p class='line' idtartingUP 2013)</p>",
	"subject": "hello",
	"timestamp": "2014-05-26 12:34:11"
	}
 ]
}
```


<aside class="success"><b>GET</b></aside>

Returns the log for a given event id

## /check_sms/{int:event_id}

> {“status”:1}

<aside class="success"><b>GET</b></aside>

Checks whether the reminder is running

Parameter | Description
--------- | -----------
status 1 | running
status 0 | not running

## /activate_sms

> {“status”: 1}

<aside class="success"><b>GET</b></aside>

activate / deactivate sms for the event

Parameter | Description
--------- | -----------
event_id | 19
sms status 1 | enabled
sms status 0 | disabled

## GET /groups/{int:event_id}

```json 
{
  "groups": [
    {
      "group_id": 1,
      "group_name": "sellers"
    },
    {
      "group_id": 2,
      "group_name": "buyers"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the Group type for a given event

Parameter | Description
--------- | -----------
event_id | 19
group_id(integer)| to identify participant group of the event 
grou_id(string)| string value associated for the specific group id 

## /terminate

> No return 

used for both text and HTML emails

**POST Parameters:**

event_id

## /html

> No return

<aside class="success"><b>GET</b></aside>

Called immediately after ‘send to all’, this API is called to blast out the email to everyone

Parameter | Description
--------- | -----------
event_id | 23
email_type | “app” or “recom” or “meetings”
cat |  ”all” or “del” or “exb” or “sin”
email |  ‘a@a.com’

## /html/details

> No return

<aside class="success"><b>POST</b></aside>

Called immediately after ‘send to all’ is pressed to verify details of the email being sent

Parameter | Description
--------- | -----------
event_id | 23
email_body |  "Hi [name]<br></br> Thanks!""
subject | "[event_name]: Networking Opportunity"
cat 1 | "all" or “new” or "ext"
cat 2 | "all" or participant_groups
cat 3 | "pending" or "not_pending" or "logged_in" or "not_logged_in" or "sent" or "not_sent" or "na" or "pending_meetings" or "meetings" or "rated" or "not_rated"
cat 4 | cat4 = "email" or "sms"

## /html/details_log

```json
{
"event": "start_processing",
"data": {
  "event_name": "GUSA 2013",
  "subject": "GUSA 2013: Call for networking"
},
"event": "communication_details",
"data": {
  "no_of_comms": 123
  },
"event": "test_report",
"data": {
  "status": "bla bla bla" 
  },
"event": "close",
"data": {
  "close" : "close stream"
  },
"event": "error",
"data": {
  "status": "database values are not filled"
}
}
```

<aside class="success"><b>GET (SSE)</b></aside>

Returns the detailed log after sending html

**STORED IN A SESSION:**

Parameter | Description
--------- | -----------
event_id | 23
email_type | “app” or “recom” or “meetings”
cat |  ”all” or “del” or “exb” or “sin”
email |  ‘a@a.com’

## /html/status

```json
{
  "event": "starting_stream", 
  "data": {
    "process": "email sender"
    },
  "event": "start_sending",
  "data": {
    "total_email": 100
    },
// on loop
  "event": "email_sent",
  "data": {
    "percentage_sent": 35.55
    },
//
  "event": "close",
  "data": {
    "close" : "close stream"
    },
  "event": "error",
  "data": {
    "error": "someone sending emails"
    },
  "event": "error",
  "data": {
    "error": "email sender crashed"
  }
{
    "eventId": "",
    "invitation":{  
        "subject": "",
        "title": "",
        "content": "",
        "button1":"",
        "instruction":{
            "section1":"",
            "section2":"",
            "section3":""
        },
        "footer":""
    },
    "schedule":{
        "subject":"",
        "instruction":{
            "section1":"",
            "section2":"",
            "section3":""
        },
        "footer":""
    }
}
}
```

<aside class="success"><b>GET (SSE)</b></aside>

Retrieves the percentage status of how many emails sent or error recd

<aside class="notice">
Variables and events should be similar to /communication/engagement/all
</aside>

**STORED IN A SESSION:**

Parameter | Description
--------- | -----------
event_id | 23
email_type | “app” or “recom” or “meetings”
cat |  ”all” or “del” or “exb” or “sin”
email |  ‘a@a.com’

# 7. /about

## /event_details

```json 
{
  "full_name": "Errol Lim",
  "account_type": 0,
  "company_name": "Jublia",
  "event": [
    {
      "id": "1",
      "name": "GUSA",
      "event_type": 0,
      //"delegate_driven": 0 //1, for delegate driven, 2 for exhibitor driven
      "post_event_leadsgen" : 0, // 1 for active
      //“group_scheduling”: 0// 1 for active
      "m_open": 1, //1 for activated
      "scan": 1 //1 for activated
    },
    {
      "id": "2",
      "name": "GUSB",
      "event_type": 1,
      "post_event_leadsgen" : 0, // 1 for active
      //“group_scheduling”: 0/ 1 for active
      "m_open": 1, //1 for activated
     "scan":1 //1 for activated
    },
    {
      "id": "3",
      "name": "GUSC",
      "event_type": 2,
      "post_event_leadsgen" : 0, // 1 for active
   //“group_scheduling”: 0/ 1 for active
     "m_open":1, //1 for activated
     "scan":1 //1 for activated
    }
  ]
}
{
  "full_name": "Errol Lim",
  "account_type": 0,
  "company_name": "Jublia",
  "event": {
  "month" : "October 2015",
  "event_list": [
    {
      "id": "1",
      "name": "GUSA",
      "event_type": 0,
      //“delegate_driven”: 0 // 1, for delegate driven, 2 for exhibitor driven
      "post_event_leadsgen" : 0 // 1 for active
      //“group_scheduling”: 0/ 1 for active
    },
    {
      "id": "2",
      "name": "GUSB",
      "event_type": 1
    },
    {
      "id": "3",
      "name": "GUSC",
      "event_type": 2
    }
  ]
 }
}
```

<aside class="success"><b>GET</b></aside>

Returns the Event Detials consisting of event type and account type for a given event

Event Type | Description
--------- | -----------
0 | leads generation
1 | speed networking (or conference)
2 | exhibitor networking (or buyer seller)
3 | hybrid

Account Type | Description
--------- | -----------
0 | organizer
1 | Engagement Managers / Organiser Admin
2 | Super Admin
3 | No database access

## /event_information/{int: eventid}

```json
  {
  "full_name": "Errol Lim",
  "event_name": "Green Urbanscape Asia 2013",
  "delegate_count": 500,
  "exhibitor_count": 80,
  "no_of_tables": 10,
  "no_of_ts": 26,
  "packages": "010"
  }
  {
  "full_name": "Errol Lim",
  "company_sname": "Jublia",
  "event_name": "Green Urbanscape Asia 2013",
  "attendee_information":
    {"group_name": "Group A",
     "group_count": 200
    },
  "event_date": "17 Jul 2015 - 18 Jul 2015",
  "no_of_tables": 10,
  "no_of_ts": 26,
  "no_of_booth": 0,
  "packages": "010"
  }
```

<aside class="success"><b>GET</b></aside>

Returns the event information of a given event specifying the type of package or plugin

Packages | Description
--------- | -----------
a | branding
b | infrastructure 
c | sponsorship

<aside class="notice">
[a][b][c] where a,b,c are either ‘0’ or ‘1’ (‘1’ means have that package)
</aside>

Plugins | Description
--------- | -----------
d | 0 (normal event), 1 (delegate-driven)
e | 0 (normal event), 1 (event is followed by leadsgen post-event)
f | 0 (normal event), 1 (group scheduling)
g | 0 (off), 1 (printing feature)

Parameter | Description
--------- | -----------
no_of_tables | is the number of tables or number of exhibitor booths (depending on type of event)
for leads_gen, no_of_tables | 'no' 
no_of_ts | 'no'

## /plan_information

```json 
{
  "full_name": "Errol Lim",
  "company_name": "Jublia Pte Ltd",
  "match_type": 1,
  "sense_type": 0,
  "total_user_count": 100
}
```

<aside class="success"><b>GET</b></aside>

Returns the plan information consisting of the name, company, plan type and number of users

Match type | Description
--------- | -----------
0 | single events
1 | multi-events
2 | multi-events +

Sense Type | Description
--------- | -----------
0 | organizer
1 | Engagement Managers / Organiser Admin
2 | Super Admin
3 | No database access

*for sense_type same as urole: (new sep 28 2015)*

<aside class="notice">
total_user_count represents total number of users who have attended all events by that company. 
</aside>

<aside class="warning">
for single events, total_user_count: ‘no’ → not doing currently
</aside>

## /latest_activity_toggle/{eventid}

```json
{
  "groups": [
    {
      "group_id": "all",
      "group_name": "All Attendee"
    },
    {
      "group_id": 1,
      "group_name": "buyers"
    }
  ],
  "toggles": [
    {
      "toggle_id": "all",
      "toggle_name": "All Activity"
    },
    {
      "toggle_id": 0,
      "toggle_name": "Sent Handshake"
    },
    {
      "toggle_id": 1,
      "toggle_name": "Received Handshake"
    },
        {
      "toggle_id": 2,
      "toggle_name": "Logged in"
    },
        {
      "toggle_id": 3,
      "toggle_name": "Handshake ignored"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the latest actions taken 

## /latest_activity/{eventid}/{page}

```json
{
  "data": [
    {
      "activity_type": "0",
      "activity_person": "XXX",
      "activity_company": "xxx",
      "activity_target": "",
      "activity_target_company": "",
      "timestamp": "DD MMM YYYY - HH:mm"
    },
    {
      "activity_type": "2",
      "activity_person": "XXX",
      "activity_company": "xxx",
      "activity_target": "yyy",
      "activity_target_company": "xxx",
      "timestamp": "DD MMM YYYY - HH:mm"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the latest activity for a given event

Logins/Handshakes | Description
--------- | -----------
0 | Login
1 | Handshake initiated
2 | Handshake returned
3 | Handshake removed

<aside class="notice">
<b>handshakes are NOT meetings made. They are handshake initiated or returned</b>
</aside>

## /create_event

> {“status” : 1,
 “event_id”: 23}

<aside class="success"><b>POST</b></aside>

Creates the event and bind it to the super admin id

Parameter | Description
--------- | -----------
event_name | “Annual Toilet Fair”
event_start | “DD/MM/YYYY”
event_end | “DD/MM/YYYY”
event_type | 1

## /reach/{event-id}/{category}

```json
{
  "nodes":[
    {"node":0,
     "name":"Total accounts",
     "xPos" : 0},
    {"node":1,
     "name": "Emails delivered",
     "xPos" : "1att"},
    {"node":2,
     "name": "Emails dropped",
     "xPos": 1},
    {"node":3,
     "name": "Emails bounced",
     "xPos" : 1},
    {"node":4,
     "name": "Accounts reached",
     "xPos" : 2},
    {"node":5,
     "name": "Accounts logged in",
     "xPos" : 3},
    {"node":6,
     "name": "Accounts not logged in",
     "xPos" : 3},
    {"node":7,
     "name": "Accounts made actions",
     "xPos" : 4},
    {"node": 8,
     "name":"Accounts not made actions",
     "xPos" : 4},
    {"node":9,
     "name": "Accounts sent handshakes",
     "xPos" : 5},
    {"node":10,
     "name": "Accounts not sent handshakes",
     "xPos" : 5}
  ],
  "links":[
    {"source":0,
     "target":2,
     "value":2},
    {"source":1,
     "target":2,
     "value":2},
    {"source":1,
     "target":3,
     "value":2},
    {"source":0,
     "target":4,
     "value":2},
    {"source":2,
     "target":3,
     "value":2},
    {"source":2,
     "target":4,
     "value":2},
    {"source":3,
     "target":4,
     "value":4}
//add 5 to 7, 5 to 8, 7 to 9, 7 to 10
- 5 to 9, 5 to 10
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns information of the number of how many people use application and sent hs with respect to the number of successful handshakes

Category | Description
--------- | -----------
1 | Person Group 1
2 | Person Group 2
3 | Person Group 3
4 | Person Group 4
5 | Person Group 5
all | Everyone

Parameter | Description
--------- | -----------
success | refers to all emails which were sent successfully
invalid | all emails which were not allowed by mandrill
empty | count of people who have no email address
not_sent | emails which have yet to be sent out
non_user_count | people who have not logged in
sent_hs_count | number of users who have sent handshakes

<aside class="notice">
Accounts usable: delivered + dropped
<p><i>note: if value is 0, remove from links</i></p>
</aside>

# 8. /analytics/leadsgen

## /connection_count/{eventid}

```json
{
  "total_connection_count": "xx"
}
```

<aside class="success"><b>GET</b></aside>

Returns a list of the amount of connections made

<aside class="warning">
<b>Parameter names are susceptible to changes</b>
</aside>

### **Calculation Method:**

(total entry in mutualmatches_core)/2

## /connection_chart/{eventid}

```json
{
  "chart_data": [
    {
      "Connection": "1", 
      "date": "21-Jan-2014"
    }, 
    {
      "Connection": "0", 
      "date": "22-Jan-2014"
    }, 
    {
      "Connection": "0", 
      "date": "23-Jan-2014"
    }, 
    {
      "Connection": "1", 
      "date": "24-Jan-2014"
    }
  ], 
  "end_date": "24-Jan-2014", 
  "start_date": "21-Jan-2014"
}
```

<aside class="success"><b>GET</b></aside>

Returns chart data for the leadsgen tab of the given event id

## /average_connection_per_day/{eventid}

```json
{
  "average_connection_per_day": "xx",
  "total_days": "xx"
}
```

<aside class="success"><b>GET</b></aside>

Returns the average connections made per day for the leadsgen tab

## /email_detail/{event_id}/{page}

```json
{
  "data": 
  {
    "email": [
      {
        "date_time": "DD/MM/YYYY 00:00",
        "sent_to": "Tan Kuan Yan, Andriano",
        "sent_by": "Shirley",
        "sub_data_1": "Hi Andriano, meet Tan Kuan Yan"
      },
      {
        "date_time": "DD/MM/YYYY 00:00",
        "sent_to": "Shirley, Andriano",
        "sent_by": "Tan Kuan Yan",
        "sub_data_1": "Hi Shirley, Hi Andriano"
      }
    ],
    "person_initiate": "Andriano",
    "company_initiate": "Exxon",
    "person_accept": "Tan Kuan Yan",
    "company_accept": "Shell"
  }
}
```

<aside class="success"><b>GET</b></aside>

Returns the email details

*Page number starts with 0*

*Each “page” has 10 threads or pairs*

# 9. /analytics/demographics

## /analytics/demographics/ event_information/{int:event_id}

```json
{
  "demographics_struct": [
    {
      "group_id": "group",
      "group_name": "Attendee Types",
      "attributes": []
    },
    {
      "group_id": 1,
      "group_name": "sellers",
      "attributes": [
        {
          "attribute": 1,
          "attribute_name": "Title"
        },
        {
          "attribute": 2,
          "attribute_name": "Location"
        }
      ]
    },
    {
      "group_id": 2,
      "group_name": "buyers",
      "attributes": [
        {
          "attribute": 1,
          "attribute_name": "Title"
        }
      ]
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

## /analytics/demographics/pie_chart/ {int:event_id}/{int:group_id}/ {int:attribute_id}

```json
{
  "data": [
    {
      "label": "\"Director\"",
      "value": 318
    },
    {
      "label": "\"Managing Director\"",
      "value": 228
    },
    {
      "label": "\"Manager\"",
      "value": 173
    },
    {
      "label": "\"General Manager\"",
      "value": 123
    },
    {
      "label": "\"CEO\"",
      "value": 120
    },
    {
      "label": "\"Sales Manager\"",
      "value": 87
    },
    {
      "label": "\"Business Development Manager\"",
      "value": 55
    },
    {
      "label": "\"President\"",
      "value": 38
    },
    {
      "label": "\"Marketing Manager\"",
      "value": 36
    },
    {
      "label": "\"Assistant Manager\"",
      "value": 35
    },
    {
      "label": "\"Engineer\"",
      "value": 29
    },
    {
      "label": "\"Executive\"",
      "value": 29
    },
    {
      "label": "\"Consultant\"",
      "value": 26
    },
    {
      "label": "\"Product Manager\"",
      "value": 26
    },
    {
      "label": "Others",
      "value": 48
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns a list of attribute_id and group_id

## /analytics/demographics/radar_chart/ {int:event_id}/{int:group_id}/ {int:attribute_id}

```json
{
  "data": [
    {
      "axes": [
        {
          "axis": "Diagnostics",
          "value": "0.142"
        },
        {
          "axis": "Electromedical Equipment / Medical Technology",
          "value": "0.187"
        },
        {
          "axis": "Medical Consumables",
          "value": "0.131"
        },
        {
          "axis": "Medical Equipment",
          "value": "0.084"
        },
        {
          "axis": "Rehabilitation Equipment / Orthopaedic Supplies",
          "value": "0.020"
        },
        {
          "axis": "Medical Furniture ",
          "value": "0.019"
        },
        {
          "axis": "Laboratory Equipment",
          "value": "0.021"
        },
        {
          "axis": "Health/Fitness",
          "value": "0.008"
        },
        {
          "axis": "Communication ",
          "value": "0.005"
        },
        {
          "axis": "Biotechnology",
          "value": "0.009"
        },
        {
          "axis": "Disinfection & Disposal Systems",
          "value": "0.000"
        },
        {
          "axis": "Pharmaceutical Supplies",
          "value": "0.060"
        },
        {
          "axis": "Services ",
          "value": "0.066"
        }
      ],
      "className": "handshake_received"
    },
    {
      "axes": [
        {
          "axis": "Diagnostics",
          "value": "0.082"
        },
        {
          "axis": "Electromedical Equipment / Medical Technology",
          "value": "0.158"
        },
        {
          "axis": "Medical Consumables",
          "value": "0.049"
        },
        {
          "axis": "Medical Equipment",
          "value": "0.129"
        },
        {
          "axis": "Rehabilitation Equipment / Orthopaedic Supplies",
          "value": "0.009"
        },
        {
          "axis": "Medical Furniture ",
          "value": "0.010"
        },
        {
          "axis": "Laboratory Equipment",
          "value": "0.003"
        },
        {
          "axis": "Health/Fitness",
          "value": "0.017"
        },
        {
          "axis": "Communication ",
          "value": "0.001"
        },
        {
          "axis": "Biotechnology",
          "value": "0.013"
        },
        {
          "axis": "Disinfection & Disposal Systems",
          "value": "0.000"
        },
        {
          "axis": "Pharmaceutical Supplies",
          "value": "0.029"
        },
        {
          "axis": "Services ",
          "value": "0.120"
        }
      ],
      "className": "handshake_sent"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

*Use*  `GET analytics/demographics/event_information/{event_id}` *for a list of attribute_id and group_id*

## Get /recover_password/{email-string}

> {“status” : 0 }

<aside class="success"><b>GET</b></aside>

Use this to recover the password from the email

Parameter | Description
--------- | -----------
0 | email does not exist
1 | email exists

# 10. /manage/scan

## scan_details/{event_id}

```json
{
 "scan_groups" : 1, 
 "total_scanners": 2, //registered on the app for that event
 "total_scans": 3,  //all scans
 "valid_scans": "20%" // valid scans as percentage
 }
```

<aside class="success"><b>GET</b></aside>

Returns the scan details for a given event 

## /scan_groups

```json
{
      "groups": [
        {
          "name": "Andriano's villa",
          "group_admin_name": "Andriano",
          "group_id": 12132,
          "booth": "B123",
          "paid": "1",
          "no_of_scanners": 4,
          "scans_count": "1000"
        },
        {
          "name": "Chinab's villa",
          "group_admin_name": "Chinab",
          "group_id": 1212,
          "booth": "",
          "paid": "0",
          "no_of_scanners": 4,
          "scans_count": "800"
        },
        {
          "name": "Mufiz's villa",
          "group_admin_name": "Mufizur Rahman",
          "group_id": 1232,
          "booth":"B123",
          "paid":"0",
          "no_of_scanners": 4,
          "scans_count": "600"
        }
      ]   
}
```

<aside class="success"><b>GET</b></aside>

Returns the scan groups for a given event

## /inner_scan_details/{event-id}/{group-id}

```json
{
  "group_name":"andriano's villa",
  "group_booth": "B123",
  "members": [
    {
      "name": "Andriano",
      "is_admin": 1,
      "scans_count": "1000"
    },
    {
      "name": "Sally",
      "is_admin": 0,
      "scans_count": "300"
    },
    {
      "name": "Sally",
      "is_admin": 0,
      "scans_count": "200"
    }
  ],
  "group_download_csv_count": [
    {
      "time": "10:00, 21st July",
      "download_member_count": "4"
    },
    {
      "time": "10:00, 21st July",
          "download_member_count": "2"
    }
  ]     
}
```

<aside class="success"><b>GET</b></aside>

Returns the inner scan details for a given event and group

## /download_group_csv/{int:group_id}

<aside class="success"><b>GET</b></aside>

# 11. /manage/matchopen

## messages/{event_id}/{category}

```json
{
 "messages": [
  {
  "message": "aadfabadda", 
  "from_email": "email_address",
  "to_exhibitor": "Google Pte Ltd",
   "message_id": "123",
   "approval_status": 1
  },
  {
  "message" : "aadfabadda", 
  "from_email": "email_address",
  "to_exhibitor": "Google Pte Ltd",
  "message_id": "123",
  "approval_status": 1
  }
 ] 
}
```

<aside class="success"><b>GET</b></aside>

Category | Description
--------- | -----------
all |
pending |
approved |
not_approved |

Approval Status | Description
--------- | -----------
0 | To be approved
1 | Approved
2 | Not approved

## /approval/{event_id}/{message_id}/ {approval status}

```json
{
"status": "marked as approved" or "marked as not approved"
}
```

> text because email can approve also and it goes to that URL and shows the above json

<aside class="success"><b>POST</b></aside>

**message_id (int)**

Approval Status | Description
--------- | -----------
1 | Approved
2 | Not approved

## /emails_details/{event_id}

```json
{
 "last_xlsx_download_date" : "22nd Feb 2012, 10:00", 
 "emails": [
   {
  "email": "email1",
  "emailid": "123456"
   },
   {
  "email": "email2",
  "emailid": "345678"
   }
  ]
 }
```

<aside class="success"><b>GET</b></aside>

Returns the email ID 

**ONLY UNIQUE EMAILS**

## /email_approval/{event_id}/{email_id}/ {delete_status}

```json
{
"status": 1
}
```

<aside class="success"><b>GET</b></aside>

**email_id (int)**

Delete Status | Description
--------- | -----------
1 | Deleted
2 | Unable to delete

## /download_emails/{event_id}

> time ordered by latest first (date time combo), 2 columns, dates and UNIQUE emails

Download the emails for a given event 

# 12. /tools/datasync

## /sync/{event_id}/{int:start_row}/ {int:end_row}/{int:participant_id}

```json
{
"event": "start_sync",
"data": {
  "status": "Initiating sync", 
  "percentage": 0
 },
"event": "report",
"data": {
  "status": "Syncing row x - x", 
  "percentage": "xx.x"
 },
"event": "end_sync",
"data": {
  "status": "Sync has been completed", 
  "percentage": 100
 },
"event": "stop_sync",
"data": {
  "status": 1,
  "rows_updated": 10,
  "rows_deleted": 11,
  "rows_inserted": 12,
  "operator": "Andriano winatra",
  "timestamp": "DD MMM YYYY - HH:mm"
 }
}
```

Syncs the db with the spreadsheet information. Performs cleaning of data as well

Status Codes | Description
--------- | -----------
0 | syncing has failed
1 | syncing is successful and statistics are returned

<aside class="notice">
NOTE: if “status” is 0, everything else will be “”
</aside>

## /sync_attendee/

```json
{
  "data": "somesuperlonghash",
  "id_attendee_login": 234234,
  "public_key": "somepublickkeystring",
  "event_id": 23,
  "fullname": "Fullname",
  "company_name": "companyname",
  "position": "position",
  "industry": "industry",
  "p_s": "",
  "p_n": ""
}
```

<aside class="success"><b>POST</b></aside>

Syncs the db with the information entered from match. Retrieves the data as if

<aside class="notice">
data is the sha512 data of the “event id,”.event_id.private_key
</aside>

## /kill_datasync/{event_id}

```json 
{
  "Status": "1"
}
```

<aside class="success"><b>GET</b></aside>

Kills the datasync for an event

## /access/{event_id}

> Return:

```json
{
 "url": "http://www.google.com"
}
```

<aside class="notice">
internal call is /access/callback
</aside>

## /activity/{event_id}

```json 
{
  "data": [
    {
      "sync_group": "All", // name
      "sync_row_start": "2",
      "sync_row_end": "4",  
      "operator": "Andriano winatra",
      "timestamp": "DD MMM YYYY - HH:mm"
    },
    {
      "rows_deleted": 11,
      "rows_inserted": 12,
      "operator": "Andriano winatra",
      "timestamp": "DD MMM YYYY - HH:mm"
    }
  ]
}
```

Gets latest ten activity on the google spreedsheet

## /ds_status/event_id

```json
{
  "status": "Syncing CMMA Visitors worksheet, row x - x",
  "percentage": 75.9,
  "ds_status": "report"
}
```

<aside class="success"><b>GET</b></aside>

Ds_status, report, error, stop_sync

## /shared_emails/{event_id}

> Data received from backend:

```json
{
  "shared_emails": [
    "errol@jublia.com",
    "nicholas@jublia.com",
    "a@gmail.com"
  ]
}
```

<aside class="success"><b>GET</b></aside>

Get the shared emails for an event

## /share

> Data returned from backend:

```json
{
   "success": "1"
}
```

<aside class="success"><b>POST</b></aside>

```json
{
"event_id" : "",
"shared_emails":  
  [
    "errol@jublia.com",
    "nicholas@jublia.com",
    "a@gmail.com"
  ]
}
```

<aside class="notice">
if success is -1, the guy did not create the document yet
</aside>

## /tools/datasync/insert/

<aside class="success"><b>POST</b></aside>

**POST Parameters:**

* event_id
* email
* full_name
* company_name
* position
* group_id

## /event_information/{event_id}

```json 
{
  "groups": [
   {
      "group_id": 0,
      "group_name": "All"
    },
    {
      "group_id": 1,
      "group_name": "sellers"
    },
    {
      "group_id": 2,
      "group_name": "buyers"
    }
  ]
  }
```

<aside class="success"><b>GET</b></aside>

Returns participant groups

# 13. /settings/general

## /event_information/{event_id}

```json
{
  "event_name": "Some Cool Event Name",
  "event_permalink_pre": "some_permalink_pre",
  "event_permalink": "some_permalink_url",
  "language": "EN",
  "event_start_date": "DD-MM-YYYY",
   "event_end_data": "DD-MM-YYYY",
  "event_timezone": "UTC+08",
  "event_location": "Batam, Indonesia",
  "dlg_em_name": "Timmy ShortFinger",
  "dlg_em_email": "tim@somevent.com"
}
```

<aside class="success"><b>GET</b></aside>

Returns event information of the given event id

<aside class="notice">
Meeting location and timeslot correspond to different column according to the type of event. The endpoint will auto detect the event type as long it is populated beforehand.
</aside>

exhibitor keywords from en_keyword

timeslot list is currently in “dd-mm-yyyy” format

available timeslots are the da

## /event_name

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Changes the Match permalink for the event

Parameter | Description
--------- | -----------
event_id | 19
name | "new_name"

Status Codes | Description
--------- | -----------
0 | permalink change has failed
1 | permalink change is successful

<aside class="notice">
Still looking into way to connect two ec2 instance
</aside>

## /timezone_offset

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Changes the event timezone

Parameter | Description
--------- | -----------
event_id | 19
timezone | "UTC+12"
location | "Batam, Indonesia"

Status Codes | Description
--------- | -----------
0 | permalink change has failed
1 | permalink change is successful

## Post /em_details

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Changes the details of the engagement manager

<aside class="notice">
use em because engagement manager is just too hard to swallow
</aside>

Parameter | Description
--------- | -----------
event_id | 20
dlg_em_name | “Timmy Shortfinger”
dlg_em_email | “tim@somevent.com”

Status Codes | Description
--------- | -----------
0 | em details change has failed
1 | em details change is successful and verification sent
2 | em details change is successful but verification send error

## /event_date

```json 
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
event_id | 26
event_start_date | “DD-MM-YYYY”
event_end_date | “DD-MM-YYYY”

## /event_language

```json 
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
event_id | 19
event_language | EN

# Sense Account Management
Part of /settings/general


## Database Changes 

> - -s_user_account.ID_COMPANY_ACCOUNT, POSITION, PUBLIC_KEY, PRIVATE_KEY, ID_EVENTS, CREDENTIALS
- -s_company_account.ID_EVENTS, UROLE, MATCH_TYPE
+ +s_user_events.ID, ID_S_USER_ACCOUNT, ID_EVENT
+ +s_company_users.ID_S_COMPANY_USERS, ID_COMPANY_ACCOUNT, ID_S_USER_ACCOUNT

<aside class="notice">
Note for Chinab: If i want to add mr abc (boss of ubm), i will go to your sense account creation page on sense, add him to any event. and then go to the database, remove off his recently added event in s_user_events. and then use his s_user_account.ID to create multiple rows in s_company_users associating all UBM comapnies with him
</aside>

* Migrate s_user_account.ID_EVENTS to s_user_events
* I have checked through sense repo, there is no more occurrence of the columns being removed (branch feature/sense_creation). Please see through that the columns are removed from database and models before production
* s_user_events.ID_EVENT = -1 implies all events access
* UROLE stuff is still done manually. So Sense only allows creation of organiser type
* Overview of the endpoints are below

## /autocomplete/"+encodeURIComponent ($(a).val())+"",

```json
{
  "data": [
  {
      "company_name": "The Montcalm at The Brewery London City;Company b",
      "id_sense_login": 124178,
      "name": "Cagri Senkul"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

## /current_company/{event_id}

```json
{"company_name": "company A"}
```

<aside class="success"><b>GET</b></aside>

Returns the company name

## /unique_companies

```json
{"company_names":["company A", "company B"]}
```

<aside class="success"><b>GET</b></aside>

## /company_name

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
company_name | Company C

## /manage_event_sense_accounts/ event_id/

```json 
{
  "data": [
  {
      "company_name": "The Montcalm at The Brewery London City",
      "id_sense_login": 124178,
       "type":1,
      "name": "Cagri Senkul",
    },
  ]
}
```

<aside class="success"><b>GET</b></aside>

Obtains all the sense accounts associated with an event

Parameter | Description
--------- | -----------
1 | Assigned (Added to the event)
2 | Permanently Assigned (Assigned via company) → don’t do anything about these when posted back (higher priority)

<aside class="notice">
find from s_user_events using EVENT_ID. Combine with
s_company_account.USER_IDS
</aside>

## /add_sense_account

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Adds a sense account associated with the event

<aside class="notice">
Cater for the case if the person already exists
</aside>

Parameter | Description
--------- | -----------
Id_sense_login | 1234
Event_id | 11

## /delete_sense_account

Deletes a Sense account

Parameter | Description
--------- | -----------
Id_sense_login | 1234
Event_id | 11

## /register_user

```json
{"status":1}
```

<aside class="success"><b>POST</b></aside>

Registers a brand new sense account

* event_id
* fullname
* email
* password

# 14. /settings/match

## /event_information/{event_id}

```json
{
 "match_start_date": "DD-MM-YYYY",
  "match_end_data": "DD-MM-YYYY",
  "match_end_time": "HH:MM",
  "handshake_disabled": 0,
  "meeting_length":"10",
  "meeting_location_amount": 20,
  "meeting_timeslot_amount": 12,
  "event_permalink_pre": "some_permalink_url",
  "event_permalink": "match_folder_name",
  "sponsor_count" : 2,
  "email_branding": 1,
  "Inapp_branding": 0,
  "lg_venue": "andrianoCRIB",
  "timeslot_list":[
    {"date": "22-12-2014", 
     "time": "10:00,10:15,10:30"},
    {"date": "22-12-2014", 
     "time": "10:00,10:15,10:30"}
  ],
  "hex_code": "xxxxx",
  "css_colour": "yellow",
  "sender_id": "xxxx",
  "disable_all_email": 1
}
```

<aside class="success"><b>GET</b></aside>

Returns event information of the given event id

<aside class="notice">
Note: meeting location and timeslot correspond to different column according to the type of event. The endpoint will auto detect the event type as long it is populated beforehand.
</aside>

exhibitor keywords from en_keyword

timeslot list is currently in “dd-mm-yyyy” format

available timeslots are the da

## Get /match_permalink

```json
{"event_permalink": "match_folder_name"}
```

<aside class="success"><b>GET</b></aside>

Returns the match permalink for the event

Parameter | Description
--------- | -----------
Event_id | 11

## /match_permalink

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Changes the Match permalink for the event

Parameter | Description
--------- | -----------
Event_id | 11
permalink | "new_permalink"
permalink_pre | "xs_link”

Status Codes | Description
--------- | -----------
0 | permalink change has failed
1 | permalink change is successful
2 | permalink has been used previously

## /meeting_location

```json
{
  "meeting_location": {
    "amount": 20,
    "label": [
      {
        "loc_id": 1,
        "loc_name": "Table 1"
      },
      {
        "loc_id": 2,
        "loc_name": "Table 2"
      }
    ]
  }
}
```

<aside class="success"><b>GET</b></aside>

```json
{
  "meeting_location": {
    "amount": 20,
    "label": [
      {
        "loc_id": 1,
        "loc_name": "Table 1"
      },
      {
        "loc_id": 2,
        "loc_name": "Table 2"
      }
    ]
  }
}
```

Returns the amount of meeting locations and their labels

Parameter | Description
--------- | -----------
Event_id | 11

<aside class="notice">
meeting_location.label - only applicable for event type 1 or 3. For others, it will be []
</aside>

## /meeting_location

```json 
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Changes the amount and label of meeting locations

assuming its only called for event type 1 or 3

Parameter | Description
--------- | -----------
event_id | 20
location_amount | 5
location_id[] | [1,2,3,4,5]
location_name[] | [“Table 1”, “Table 2”, “Table 3”, “Table 4”, “Aloha table”]

Status Codes | Description
--------- | -----------
0 | table amount change has failed because database error
1 | table/booth amount change is successful
2 | table amount cannot be reduced because the platform is launched and the tables need to be reduced

## /ig_venue

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Creates new venue

Parameter | Description
--------- | -----------
event_id | 20
ig_venue | "My own bed"

## /event_timeslots

<aside class="success"><b>POST</b></aside>

Creates new event timeslots

Parameter | Description
--------- | -----------
event_id | 20
date_list | [“12-01-2014”, “13-01-2014”]’,
time_list | [“]

## /event_match

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Creates a new match event

Parameter | Description
--------- | -----------
event_id | 20
event_match_start | “DD-MM-YYYY”
event_match_end | “DD-MM-YYYY”
event_match_end_time | ”HH:MM”

## /add_handshake_blocked

```json
{"status" : 1}
```

Adds a blocked handshake

Parameter | Description
--------- | -----------
event_id | 20
add_handshake_blocked | 1

## /sponsor_count

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Creates a Sponsor counter

Parameter | Description
--------- | -----------
event_id | 20
sponsor_count | 0

## /email_branding 

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Creates email branding for an event

Parameter | Description
--------- | -----------
event_id | 20
email_branding | 1

## /inapp_branding

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Creates branding in the APP

Parameter | Description
--------- | -----------
event_id | 20
inapp_branding | 1

## /app_colour

```json
{"status" : 1}
```

<aside class="success"><b>POST</b></aside>

Gives the APP some colour

Parameter | Description
--------- | -----------
event_id | 20
hex_code | “xxxxxx”
css_colour | ”yellow”

## /sender_id

<aside class="success"><b>POST</b></aside>

Creates a Sender ID for a given event 

Parameter | Description
--------- | -----------
event_id | 20
sender_id | “xxxxxx”

## /disable_all_email

<aside class="success"><b>POST</b></aside>

Disables all email from being sent

Parameter | Description
--------- | -----------
event_id | 20
disable_all_email | 0/1

## /branding_image

```json
{
"status": 0 or 1 or 2
}
```

<aside class="success"><b>POST</b></aside>

Saved the branding image for the event

Parameter | Description
--------- | -----------
event_id | 20
type | “banner-logo” or “inapp-banner” or “spon” 
file | “no idea what the values are”

Status Codes | Description
--------- | -----------
0 | unable to save
1 | saved
2 | incorrect dimensions

# 15. /settings/datascheme

## /groups_info/{int:event_id}

```json
{
  "data": [
    {
      "group_id": 1,
      "group_name": "Delegate",
      "exhibitor_group": 0,
      "open_group": 0, 
      "group_led" : 0,
      "view_list": "2,3",
      "public_name": "Delegate", // used in email
     "public_group_name": "Delegate" // used in match
    },
    {
      "group_id": 2,
      "group_name": "Exhibitor",
      "exhibitor_group": 1,
      "open_group": 1, 
      "public_name": "Delegate",
      "view_list": "1, 3"
    },
    {
      "group_id": 3,
      "group_name": "Trade Visitor",
      "exhibitor_group": 0,
      "open_group": 0, 
     "public_name": "Delegate",
      "view_list": 1
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns the group infomation for a given event

## /write_groups

```json
{
  "event_id": 45,
  "group_id": [
    1, 2, 3
  ],
  "group_name": [
    "Delegate",
    "Exhibitor",
    "TV"
  ],
  "exhibitor_group": [
    0, 1, 0
  ],
  "open_group": [
    0, 1, 0
  ],
  "view_list": [
    "2,3", "1,3", "1"
  ],
  "public_name": [
    "Delegate group",
    "Exhibitor crib",
    "TV team"
  ],
 "public_group_name":["Delegate",...]
}
"group_led": 0, 1, 2, 3

{"status": 1}
```

<aside class="success"><b>POST</b></aside>

1 is for success

0 is for failure

## /attributes_info/{int:event_id}

```json
{
  "data": [
    {
      "attribute_id": 1,
      "attribute_name": "Industry",
      "separator_required": 0,
      "titalisation_required": 1,
      "dossier_required": 1,
      "tweetpitch_required": 1,
      "keyword_required": 1,
      "interest_required": 1,
      "show_single_counts": 1,
      "outer_profile_required": 1,
      "scan_csv_required": 1
    },
    {
      "attribute_id": 2,
      "attribute_name": "Products",
      "separator_required": 1,
      "titalisation_required": 0,
      "dossier_required": 1,
      "tweetpitch_required": 1,
      "keyword_required": 1,
      "interest_required": 0,
      "show_single_counts": 1,
      "outer_profile_required": 0
    },
    {
      "attribute_id": 3,
      "attribute_name": "Location",
      "separator_required": 1,
      "titalisation_required": 1,
      "dossier_required": 1,
      "tweetpitch_required": 1,
      "keyword_required": 1,
      "interest_required": 1,
      "show_single_counts": 0,
      "outer_profile_required": 0
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Get the attribute information for a given event 

## /write_attributes

```json 
{"status": 1}
```

<aside class="success"><b>GET</b></aside>

Parameter | Description
--------- | -----------
event_id | 20
attribute_id | 1, 2, 3
attribute_name | "Industry", "Products", "Location"
separator_required | 0, 1, 1
titalisation_required | 1, 0, 1
dossier_required | 1, 0, 1
tweetpitch_required | 1, 0, 1
keyword_required | 1, 0, 1
interest_required | 1, 0, 1
show_single_counts | 1, 0, 1
outer_profile_required | 1, 0, 1
scan_csv_required | 1, 0, 1

1 is for success

0 is for failure

## /event_information/<event_id>

```json 
{ "event_id": 60,
  "keyword": [
    {
      "participant_group_name": "delegate",
      "participant_group_id": 0,
      "participant_keyword": [
        {
          "category_title": "Title",
          "category_value": [
            "Manager",
            "Director"
          ]
        },
        {
          "category_title": "Industry",
          "category_value": [
            "Event Services",
            "Banking"
          ]
        }
      ]
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

<aside class="warning">
ONLY returns event_keyword stuff
</aside>

## /event_keywords/

<aside class="success"><b>POST</b></aside>

Create keywords for an event

Parameter | Description
--------- | -----------
event_id | 20
categories | [1_position, 1_industry] // the format is <participant_group_id>_<attribute_name>
category_value | []

## fresh_event_keywords/{int:event_id}
> same as /event_information

<aside class="success"><b>GET</b></aside>

Returns fresh keywords for an event

# 16. /recover_password

## /recover_password/<email_string>

```json
{"status": 1}
```

<aside class="success"><b>GET</b></aside>

Returns the password for a given event

Parameter | Description
--------- | -----------
status 1 | User exists(this will send the recover email)
status 0 | Not an user (this will not send any email)

# 17. /tools/crm

## /autocomplete/event_id/{word}

```json 
{
  "data": [
  {
      "company_name": "The Montcalm at The Brewery London City",
      "email": "cagri@themontcalmlondoncity.co.uk",
      "id_attendee_login": 124178,
      "name": "Cagri Senkul",
      "position": "Brewer",
      "Language": "CN",
      "Contact_no": "6593708258"
    },
  ]
}
```

<aside class="success"><b>GET</b></aside>

Returns autocomplete details for an event

## /requests_breakdown/ {int:id_attendee_login}/ {int:requests_required}

```json
{
  "data":
  {
    "category_name": "some_name", 
    "category_sub": "some_company", 
    "person_type": "1",
    "handshake_status": "1",
    "made_action": "0"
  }
}
```

<aside class="success"><b>GET</b></aside>

Returns a breakdown of requests made by attendees

Requests Required | Description
--------- | -----------
0 | sent
1 | received
2 | meeting
3 | Bookmarked
4 | Archived

## /attendee_statistics/{id_attendee_login}/ {attendee_status}/{delete_reason}

```json 
{
"event": "start_delete",
"data": {"status": "Initiating delete"},

"event": "report",
"data": {"status": "removing meetings"},

"event": "end_delete",
"data": {"status": "delete has been completed"}
}
```

<aside class="success"><b>GET</b></aside>

Returns the statistics of an attendee consisting of the event attended and the meetings and reasons they rejected

* id_attendee_login
* attendee_status
* delete_reason

Attendee Status | Description
--------- | -----------
0 | active
1 | deleted
2 | hidden
3 | PERMANENT DELETED


Reject Reasons | Description
--------- | -----------
1-1 | Not Attending
1-2 | Don't want to schedule meetings
1-3 | Prefer to just walk around
1-4 | Only there for content (talks/speaking)
1-5 | Visa issues
1-6 | Removed from API

## /change_attendee_notification

```json
{"status": 1}
```

<aside class="success"><b>POST</b></aside>

Creates notification changes for an attendee

* id_attendee_login
* notification_state

Notification State | Description
--------- | -----------
0 | no emails / disable
1 | summary
2 | real-time notifications / immediate

## /attendee_email_status/ {int:id_attendee_login}

```json 
{"notification_state": 1,
  "email_status": 1,
  "email_status_string": "whatever error message",
  "email_list":
      {"email_subject": "Email subject",
       "Email_body": "Emailbody",
       "Email_opened": 1,
       "Timestamp": "DD MMM YYYY - HH:mm"}
     }
```

<aside class="success"><b>POST</b></aside>

Creates an attendee email containing the attendee's status

Email Status Code | Description
--------- | -----------
1 | Success
2 | Spam
3 | Empty
4 | Soft Bounce
5 | Hard Bounce
6 | Unsubscribe

<aside class="warning">
attendee_Login Status (not to be confused with attendee_status), yes I know, it’s confusing, deal with it
</aside>

# 18. /tools/testing

**NOTE FOR DB:**

**TEST_TYPE:**

1. general_settings
2. match_settings
3. groups_attributes_settings
4. primary_inspection
5. secondary_inspection

## /match/{int:event_id}

```json
{
"general_settings": 1,
"match_settings": 1,
"groups_attributes_settings": 2,
"primary_inspection": "Verified by Tan Kuan Yan at [timestamp]",
"secondary_inspection": 2
}
```

<aside class="success"><b>GET</b></aside>

Overall check when Match testing page is loaded

Parameter | Description
--------- | -----------
0 | test failed
1 | test is successful
2 | test wasn’t carried out

## /match/check

```json
{
"status": 0 or 1
}
```

<aside class="success"><b>POST</b></aside>

Runs the test and obtains the status and report associated with test_type settings

Parameter | Description
--------- | -----------
event_id | 19
test_type | “general” or “match” or “groups_attributes”

**Return**

Parameter | Description
--------- | -----------
0 | Error detected
1 | Test passed or warnings

## /match/log/{int:event_id}/{str:test_type}

```json 
{
  "log": [
    {
      "tester": "Andriano winatra",
      "test_status": 0,
      "test_report": "hello wassup <br/> hi",
      "timestamp": "2014-05-26 12:34:12"
    },
    {
      "tester": "Andriano winatra",
      "test_status": 0,
      "test_report": "hello wassup <br/> hi hi",
      "timestamp": "2014-05-26 12:34:13"
    },
    {
      "tester": "Sherlly Septiani",
      "test_status": 1,
      "test_report": "works",
      "timestamp": "2014-05-26 12:34:14"
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Runs the test and obtains the status and report associated with test_type settings

Parameter | Description
--------- | -----------
test_type | “general”, “match”, “groups_attributes”

## /match/inspection

```json
{"status": "Verified by Tan Kuan Yan at [timestamp]"}
```

<aside class="success"><b>POST</b></aside>

EMs need to carry out a visual inspection to ensure correctness of the platform

Parameter | Description
--------- | -----------
event_id | int
type | int
1 | primary EM check
2 | secondary EM check

# 15. /tools/agenda

## /tools/agenda/settings/{int:event_id}

```json
{
  "description": "sads",
  "end_date": "2016-11-30",
  "event_id": 340,
  "start_date": "2016-11-25",
  "venue": "tes tets test test"
}
```

<aside class="success"><b>GET</b></aside>

Returns information about the agenda from Agenda_Settings table

Parameter | Description
--------- | -----------
event_id | int

## /tools/agenda/settings/{int:event_id}

> If the Data gets updated

```json 
{"status":1}
```

> else

```json 
{"status":0}
```

> 

```json 
{"status":401} // Start Date cannot be later than End Date
```

<aside class="success"><b>POST</b></aside>

Post data to Agenda_Settings

Parameter | Description
--------- | -----------
event_id | (int)
venue | (str)
description | (str)
start_date (datetime) | yyyy-mm-dd
end_date (datetime) | yyyy-mm-dd

## /tools/agenda/settings/toggle_published/ {int:event_id}

<aside class="success"><b>POST</b></aside>

Creates a toggle to publish an event

Parameter | Description
--------- | -----------
event_id | (int)
is_enabled | (int)

## /tools/agenda/settings/days/{int:event_id}

```json
{
  "agenda_day_track": [
    {
      "day": 1,
      "date": "25 Nov 16",
      "id_agenda_day": 708,
      "track": []
    },
    {
      "day": 2,
      "date": "26 Nov 16",
      "id_agenda_day": 709,
      "track": [
        {
          "track_description": "Fake Description",
          "track_id": 46,
          "track_location": "Fake Venue",
          "track_name": "Track_1",
          "track_status": 1
        }
      ]
    },
    {
      "day": 3,
      "date": "27 Nov 16",
      "id_agenda_day": 710,
      "track": []
    },
    {
      "day": 4,
      "date": "28 Nov 16",
      "id_agenda_day": 711,
      "track": [
        {
          "track_description": "Tech",
          "track_id": 47,
          "track_location": "Suite G",
          "track_name": "Second Track",
          "track_status": 1
        },
        {
          "track_description": "Finance",
          "track_id": 48,
          "track_location": "Suite F",
          "track_name": "Third Track",
          "track_status": 1
        }
      ]
    },
    {
      "day": 5,
      "date": "29 Nov 16",
      "id_agenda_day": 712,
      "track": []
    },
    {
      "day": 6,
      "date": "30 Nov 16",
      "id_agenda_day": 713,
      "track": [
        {
          "track_description": "Gold",
          "track_id": 49,
          "track_location": "Diamond",
          "track_name": "Trial",
          "track_status": 1
        }
      ]
    }
  ]
}
```

<aside class="success"><b>GET</b></aside>

Get the all days and corresponding tracks in one json list

Parameter | Description
--------- | -----------
event_id | (int)

## /tools/agenda/settings/track_delete

> If delete successful, returns:

```json
{"status": 1}
```

> else

```json
{"status": 0}
```

<aside class="success"><b>POST</b></aside>

Delete a specific track using id_agenda_track

Parameter | Description
--------- | -----------
track_id | (int)

## /tools/agenda/settings/track_save

```json 
{"status": 1}
```

<aside class="success"><b>POST</b></aside>

Create or update track

Parameter | Description
--------- | -----------
id_agenda_day | (int)
id_agenda_track[] | (array)
description[] | (array)
name[] | (array)
location[] | (array)
event_id | (int)

## /tools/agenda/builder/full_agenda_details/ session/shift

> If commit successful, returns: 

```json
{
  "end_time": 730,
  "id_agenda_session": 1,
  "id_agenda_track": 49,
  "start_time": 630
}
```

> else

```json 
{"status": 0}
```

<aside class="success"><b>POST</b></aside>

Update session attribute and returning them in the same response

Parameter | Description
--------- | -----------
id_agenda_session | (int)
new_start_time 
new_end_time 
new_track_id

## /tools/agenda/builder/full_agenda_details/ session/{int:id_agenda_session}

```json 
{
  "date": "15 Dec 16",
  "description": "",
  "end_time": "11:00",
  "id_agenda_session": 132,
  "is_live": 0,
  "location": "",
  "start_time": "10:30",
  "tags": [],
  "title": "asdada",
  "track_name": "asda"
}
```

<aside class="success"><b>GET</b></aside>

Get session details

Parameter | Description
--------- | -----------
id_agenda_session | (int)

## /tools/agenda/builder/full_agenda_details/ session/new

```json
{
  "end_time": 780,
  "id_agenda_session": 50,
  "start_time": 690,  
  "status": 1
}
```

> Failure {“status”: 403} // End Time should not be earlier than Start Time

> Failure {“status”: 404} // Session with same start_time exist

<aside class="success"><b>POST</b></aside>

Create a new session

Parameter | Description
--------- | -----------
id_agenda_session | (int)
title | (str)
description | (str)
start_at | (str) // 11:30
end_at | (str) // 12:00
location | (str)
event_id | (int)
tag_values[] | (array)

## /tools/agenda/builder/full_agenda_details/ session/{int:id_agenda_session}

> Success:

```json 
{"status": 1}
```

> Fail

```json
{"status": 0}
```

<aside class="success"><b>DELETE</b></aside>

Delete an existing session

Parameter | Description
--------- | -----------
id_agenda_session | (int)



## /tools/agenda/builder/full_agenda_details/ session

> Success 

```json
{
  "end_time": 720,
  "id_agenda_session": 57,
  "start_time": 690
}
```

> Fail

```json 
{"Status": 0}
```

<aside class="success"><b>POST</b></aside>

Updates Session

Parameter | Description
--------- | -----------
id_agenda_session | (int)
title | (str)
description | (str)
start_at | (str)
end_at | (str)
location | (str)
event_id | (int)
tag_values | (array)

## /tools/agenda/builder/full_agenda_details/ {int:event_id}

```json
{
  "agenda_days": [
    {
      "day": 1,
      "date": "26 Nov 2016",
      "id_agenda_day": 709,
      "track": [
        {
          "id_agenda_track": 46,
          "name": "Track_1",
          "session": [
            {
              "description": "Desc Test",
              "id_agenda_session": 4,
              "is_live": 0,
              "location": "Diamond",
              "name": "Test",
              "speakers": "",
              "tags": [],
              "time_end": 689,
              "time_start": 660
            },
            {
              "description": "test desc",
              "id_agenda_session": 8,
              "is_live": 0,
              "location": "singapore",
              "name": "test session",
              "speakers": "",
              "tags": [],
              "time_end": 860,
              "time_start": 830
            },
            {
              "description": "latest test desc",
              "id_agenda_session": 46,
              "is_live": 0,
              "location": "singapore",
              "name": "latest test session",
              "speakers": "",
              "tags": [
                "malaysia",
                "china",
                "USA"
              ],
              "time_end": 630,
              "time_start": 600
            }
          ]
        }
      ]
    },
  ],
  "total_tracks": 4
}
```

<aside class="success"><b>GET</b></aside>

Return all days, tracks, sessions, tags and speakers under one JSON list

Parameter | Description
--------- | -----------
id_agenda_session | (int)

## /tools/agenda/builder/full_agenda_details/ session/venue

```json
{
  "key": [
    "Suite G",
    "singapore"
  ]
}
```

<aside class="success"><b>POST</b></aside>

Returns a maximum of 5 related keywords location

Parameter | Description
--------- | -----------
id_agenda_session | (int)
event_id | (int)
keyword | (str)

## /tools/agenda/builder/session/ toggle_published

> Succeed:

```json
{"status": 1}
```

> Fail:

```json
{"status": 0}
```


Set Session active or inactive

Parameter | Description
--------- | -----------
id_agenda_session | (int)
is_live | (int) // 1(active) or 0(inactive)

## /tools/agenda/builder/full_agenda_details/ session/tag

```json
{
  "tags": [
    "Startups"
  ]
}
```

Returns a maximum of 5 related keywords location

Parameter | Description
--------- | -----------
event_id | (int)
keyword | (str)

## /tools/agenda/builder/full_agenda_details/ speaker/new

> Succeed:

```json
{"status": 1}
```

> Fail:

```json
{"status": 0}
```

<aside class="success"><b>POST</b></aside>

Creates a new speaker

Parameter | Description
--------- | -----------
email | (str)
fullname | (str)
companyname | (str)
position | (str)
profile | (str)
companydescription | (str)
image_url | (str)
event_id | (int)
id_agenda_speaker | (int)

## /tools/agenda/speaker

> Succeed

```json
{"status": 1}
```

> Fail

```json
{"status": 0}
```

<aside class="success"><b>DELETE</b></aside>

Set speaker.is_active to 0, delete all relating session_speaker

Parameter | Description
--------- | -----------
id_agenda_session | (int)

# Match/Process API Bridge

## /sync_acquisition

<aside class="success"><b>POST</b></aside>

Syncs the acquisition

Parameter | Description
--------- | -----------
event_id | event id
data | hashed data
public_api_key | public api key
updated_id_1 | id 
updated_id_2 | id

## /sync_meeting or /sync_rating

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
event_id | event id
public_api_key | public api key
first_person | id of first person
second_person | id of second person
id_timeslot | id timeslot, -1 for removal of pending
id_table | table of the meeting
pending_status | 1, to refresh pending

## /sync_attendee

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
event_id | event id
id_attendee_login | attendee id
fullname | full name of the attendee
companyname | company name of the attendee
position | position of the attendee
industry | industry of the attendee
country | country of the attendee
company_url | company url
company_desc | company desc

## /sms_meeting

<aside class="success"><b>POST</b></aside>

Creates a SMS message to alert attendees of meetings

Parameter | Description
--------- | -----------
event_id | event id
public_key | public api key 
data | hashed data
sent_id | the inititaor
other_id | the victim
location | location of the meeting
time | time
type | type of sms

<aside class="notice">
‘add’ or ‘cancel’ or ‘resched’
</aside>

## /sync_profile

```json
{
"status": 0 or 1 (int)
}
```

<aside class="success"><b>POST</b></aside>

Parameter | Description
--------- | -----------
event_id | event id
id_attendee_login | id of the person
full_name | name of the person
company_name | company_name
position | position
company_url | company_url
company_desc | company description
p_s | personal summary
cc | country code
p_n | phone number

# reminder_system.py 

`python reminder_system.py <event_id> <starting_timeslot>`

*e.g : python reminder_system.py 11 0*


# event_sms.py

`python event_sms.py <event_id> <target>`

*e.g : python reminder_system.py 11 delegate*


#feedback_sms.py

`python feedback_sms.py <event_id> <day>`

*e.g : python feedback_sms.py 11 1*

<aside class="notice">
NOTE: day always starts with 1
</aside>

# Standalone scripts

scripts listed below are able to run without sense

# titlize_attribute.py

### **Function:** 

titilize attributes of attendees

### **Parameter:** 

python titilize_attribute.py <event_id>

*e.g: python titilize_attribute.py 11*


# scrape_domain.py

### **Function:** 

generate company description based on email given

### **Parameter:** 

python scrape_domain <event_id>

*e.g: python scrape_domain 11*


# generate_company_tags.py

### **Function:**

generate company tags based on company description

### **Parameter:**

python generate_company_tags.py <event_id>

*e.g: python generate_company_tags.py 11*


#generate_search_terms.py

### **Function:** 

generate search terms based on attributes

### **Parameter:**

python generate_search_terms.py <event_id>

*e.g: python generate_search_terms.py 11*

<aside class="notice">
the ones with en are for exhibitors because it contains dlg data
the ones without en are for delegates because they contains exb data
</aside>

# post_upload.py

### **Function:**

runs all of the above four scripts

### **Parameter:**

python post_upload.py <event_id>

*e.g: python post_upload.py*


# cron_job_populate_ standalone.py

### **Function:** 

updates the json for meetings on demand / in cron

### **Parameter:**

 python cron_job_populate_standalone.py <event_id>

*e.g: python cron_job_populate_standalone.py 11*


#upload_script.py

### **Function:** 

despite the name, it actually syncs the db with google spreadsheet

### **Parameter:**

python upload_script.py <event_id>

*e.g: python upload_script.py 11*

# DB PLUGINS COLUMN

**[ 0 ] [ 0  ]**

No. of locales: table, booth

the plugins column in event info determine what kind of special attributes does the event have.

The first number determines whether the event is a delegate driven event

The second number determines whether the event has a leadsgen continuation after the initial event

**UROLE**

0. ORGANIZERS
1. EMS
2. ADMINS
3. NO DB ACCESS

# Testing workflow to launch an event

### 1. Create event in sense 

* edit data in event_info row for type of event and number of tables
* check attribute_terms + timestamp_lookout + location_reference
* go to generals and fill the up the remaining data
* Check match_settings, rate_settings, engagement_settings
* if event is branded go to db and the colors yourself, change the "Packages" column
* if event type has a post leadsgen or delegate driven or group scheduling, change the "plugins" column. Sponsors
* (Match) copy files manually over to permalink
* participant_group, atttribute_group, POSITION\

### 2. Add data via datasync
* add attendee data via csv import or manual copy paste
* sync(if successful ids are generated in the column) and proceed to change the company description and company_url that looks weird
* sync again to make sure they were changed
(regarding event_keywords and search terms, report to andriano if there could be improvements)
* skim through event_keywords in general/settings
* skim through search_terms in s_temp_json
* make sure every attendee has their passcodes via DB
* make sure the comma separation is without space

### 3. Test event
* lost passcode
* intend to meet for e and d
* refresh list
* revpie page 2
* search for e and d
* archive (Search)
* bookmark (Search)
* logout summary / logout with 1 handshake summary / logout page
* schedule email
* add hs / remove hs / return hs (normal or delegate driven) / (acquisition test)
* add sync_attendee test

### 4. Send a test of app email to EM, chinab, organizers (if required)

### 5. Sense account for organizers

Noise filter for scraper results.

Noise filter is now implemented for scraper. Noise filter uses points and relevance to return the best results out of a result set supplied by Bing search.


The process of filtering starts with assigning points to each of the result set. Points themselves are divided into two categories, base points which is the starting point of each result set and additional points which are added according the cases of each result set. Base points always start at 100


Base points assigned differ according to the cases provided below:

* Case 1: If company name exists in the cautionary word list, base points are increased by 8

* Case 2: If company name exists in the critical word list, base points are increased by 15


Additional points assigned differ according to the case(s) provided below:

* Case 1: If result set URL contains the word “about” or wikipedia, the addtional points given is 21


After each results set have been assigned points, it is then passed through filters. Each filter has their own category of scoring. Currently, there are only 3 filters:

## Number filter

The number filter counts occurrences of numbers and subtract points accordingly. 

The general rules of the number filter are below:

* Number occurences that only have 1 number is exempted
* Number occurences that only have 4 number is exempted
* Number occurences other than stated above are counted as violations
* The threshold of the filter is 2 number occurences
* For each occurences that passes the threshold, -2  is applied to the points

*Example of the filtering process :*

result set JSON:

`result_set = {“description”: “george has 12 cups in 2014 and also 20 plates and 20 forks”, “points”: 100}`

from the above result set, we can see that the particular description has 4 number instances, 3 of them containing 2 numbers and 1 that contains 4 numbers. The one that has 4 numbers which is “2014” is exempted from the filtering. Now we have 3 number occurrences that violates the rules of the filter. The threshold of the number filter is 2, so what we are left is 1 number occurrence.
 
Since for each violations over the threshold, the points is subtracted by 2 the end result of the filter is :

`result_set = {“description”: “george has 12 cups in 2014 and also 20 plates and 20 forks”, “points”: 98}`

## Words filter

The words filter counts occurences of categories of words and subtract points accordingly. Currently there are two category of words that are viewed to be obstructive:

* Critical: “powered by”, “sponsored link”
* Cautionary: “jobs”, "news", "website", "search engine", "likes", "is located", "Facebook", " copyright", "adobe", "joomla", "linkedin"

The general rules of the words filter are noted below:

* each critical words will subtract points by 15
* each cautionary words will subtract points by 8
multiple same words will not result in multiple subtraction


## Symbols filter

**Old event type:**

0. leads generation
1. speed networking (or conference)
2. exhibitor networking (or buyer seller)
3. speed networking followed by leadsgen
4. exhibition (at speed networking tables)
5. exhibition (delegate-driven)
6. hybrid

**attendee _Login Status (not to be confused with attendee_status):**

<aside class="warning">
 yes I know, it’s confusing, deal with it
</aside>

1. Success
2. Spam
3. Empty
4. Soft Bounce
5. Hard Bounce
6. Unsubscribe

**Attendee_login notification_state:**

0. no emails / disable
1. summary
2. real-time notifications / immediate








