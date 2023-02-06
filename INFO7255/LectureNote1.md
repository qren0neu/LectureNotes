## Introduction to the topic

What is strongly typed?

Key / Values stores

REST API

How did people come up with GraphQL?

**Difference and Common between REST API and GraphQL.**

Main Topics Covered:

1. Strongly Typed Data & Key / Value
2. REST  API
3. Security
4. GraphQL
5. Search

## Why Big Data

[A short history of the web](https://home.cern/science/computing/birth-web/short-history-web)

On 1993 and 1994, the web became globally.

When web search, social, email become a hub (like google, facebook, LinedIn), the scale became millions times than before.

The millions of requests came continuously which make us have to think about everything into a completely different way.

Why Big Data?

1. Volume
	1. The amount of data is large
2. Variety 
	1. The different types of data, you have to be able to search through it
		1. Audios
		2. Videos
		3. Free text (Compared to highly structured data like XML or Relational Database)
3. Velocity
	1. Consider the "Spike", if your system is designed for 100,000 requests per second, there will be a spike that comes 200,000 requests per second
	2. The velocity of the data changes
4. Extensibility (Schemaless)
	1. We make a system to meet the requirements, but the requirements always change
	2. We may have to make a system that is not strongly tied to a schema, or use "schemaless"

## Use Case

A company wishes to provide its employees medical coverage. So, they create medical plans tailored to the employees needs. Each plan consists of large number of covered services, e.g. acupuncture, physical, well-baby visits, emergency room visits, and so on. Additionally, each plan specifies the cost associated with that plan. For example, the co-pay for the various visits, and any deductible that should be met before the patient is reimbursed.

the company has created a website for its employees where they can view each medical plan and the covered services associated with the plans. Additionally, the website is also used by the plan administrators to create new plans and modify existing plans.

Is this a use case of big data?

It depends on the volume of the employees and the amount of medical plans (Maybe 10,000 employees and 10 plans)

Also, suppose the system is as follows.

Our system provides insurance solutions to many customers, we may have 50 insurance companies, and over time they may create thousands of different solutions. And there will be such a time in the year when there may be a large influx of orders from large companies that are hiring or for whatever reason. In addition, the system needs to send these client companies the orders they have in bulk for a specific period of time each day.

Also, depending on the company, how big will a medical plan be? Some companies may be 100k (but you have to consider 500k), but then there are companies that will have 3MB.

For example:

```json
{

	"planCostShares": {
		"deductible": 2000,
		"_org": "example.com",
		"copay": 23,
		"objectId": "1234vxc2324sdf-501",
		"objectType": "membercostshare"
		
	},
	"linkedPlanServices": [{
		"linkedService": {
			"_org": "example.com",
			"objectId": "1234520xvc30asdf-502",
			"objectType": "service",
			"name": "Yearly physical"
		},
		"planserviceCostShares": {
			"deductible": 10,
			"_org": "example.com",
			"copay": 0,
			"objectId": "1234512xvc1314asdfs-503",
			"objectType": "membercostshare"
		},
		"_org": "example.com",
		"objectId": "27283xvx9asdff-504",
		"objectType": "planservice"
	}, {
		"linkedService": {
			"_org": "example.com",
			"objectId": "1234520xvc30sfs-505",
			"objectType": "service",
			"name": "well baby"
		},
		"planserviceCostShares": {
			"deductible": 10,
			"_org": "example.com",
			"copay": 175,
			"objectId": "1234512xvc1314sdfsd-506",
			"objectType": "membercostshare"
		},
		
		"_org": "example.com",
		
		"objectId": "27283xvx9sdf-507",
		"objectType": "planservice"
	}],


	"_org": "example.com",
	"objectId": "12xvxc345ssdsds-508",
	"objectType": "plan",
	"planType": "inNetwork",
	"creationDate": "12-12-2017"
}
```

Assuming a 3MB json plan with 5,000 such objects, we might have to write to the database 5,000 times in a second. (plus catching a peak, this number of writes could be incredibly high)

>[!information]
>If you want to know the use case is a Big Data case or not, you will have to ask many questions.

### How to judge?

Start by asking a few questions:
what is the data size of a medical plan?
What does a medical plan look like? That is, how can we model a medical plan?

Other factors?
How many people are viewing the website? E.g. throughput rates, latency requirements?

Is there a need to batch import/export plans from the system?

### What's more 

The analyst responsible for plan creation wants to quickly modify any plans that he created with additional attributes. For example, the analyst may want to remove services and add services to the plan. The analyst may also wish to extend any plan with additional attributes that may not have been foreseen during the design of the system.

The question is: how can we extend the definition of a plan?

---
While an analyst editing a plan, this plan must not be visible to employees. Furthermore, other analysts may view, but not edit, this plan

Hence, the need to secure the system with authentication, and authorization support

---
An employee using the system may find the medical plan that best fit his/her needs by using the search box. A user may search on any attribute

Technical requirement:
need for search

## Technical requirements so far

- Need for data modelling
- Need for CRUD APIs
- Need for batch APIs
- Need for data extensibility
- Need for data validation

## After class

Json schema: http://json-schema.org/

Json: HTTP://json.org

### Why JSON Schema?

We need to validate the JSON.

Here is an example using the JSON above.

```json
{
	"_org": "example.com",
	"objectId": "12xvxc345ssdsds-508",
	"objectType": "plan",
	"planType": "inNetwork",
	"creationDate": "12-12-2017"
}
```

We know here that `creationDate` is a String, so we need a `schema` to do the validation: the type of `creationDate` must be String.
