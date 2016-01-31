---
title: CharityAPI Documentation

language_tabs:
  - python
  - ruby


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to CharityAPI. This API connects you to several IRS datasets on nonprofit organizations. It updates biweekly, to provide the most up-to-date information, in the most convenient way possible, freeing nonprofits to spend their time and resources on their mission.

#What can I do with this data?
CharityAPI makes it easy for non-profits, groups supporting non-profits, and grant dispersing organizations to verify the the status of non-profit organizations. It also provides a tool to do analyses on spending across charitable sectors.
-Authenticate clients and users as non-profit organizations real-time
-Assess spending in different non-profit sectors
-Look at the geographical distribution of spending
-Collect longitudinal data on assets and revenues

#Data Sources
**All data come from the Internal Revenue Service.**

* <a href='https://github.com/tripit/slate'>
Documentation on many of the data elements</a>

* <a href='https://www.irs.gov/Charities-&-Non-Profits/Exempt-Organizations-Business-Master-File-Extract-EO-BMF'>
Organization-data</a>

* <a href='https://apps.irs.gov/app/eos/pub78Search.do?searchChoice=ePostcard&dispatchMethod=selectSearch'>
IRS Exempt Organizations Select Check </a>

# Authentication
CharityAPI is currently open, but you will soon need an API keys to access the API. You will be able to register for a new key at our [developer portal](http://charityapi.org/developers).

Use the API key in all API requests to the server as a parameter that looks like the following:

`Authorization: your_key_here`

<aside class="notice">
You must replace <code>your_key_here</code> with your personal API key.
</aside>

# Look up an EIN

The API currently allows you to look up a single ein at a time, and returns a json with all available data for that organization. Fields are listed in the data section of this documentation. To batch process, iterate through a list of ein strings. 

The http request is: https://charityapi.org/api/<ein>


```ruby

require 'rest_client'
EIN_Data = RestClient.get 'http://charityapi.org/api/363673599' 


```

```python
import requests

url='http://charityapi.org/api/'

ein='363673599'

EIN_Data=requests.get(url+ein)


```

> The above command returns JSON structured like this, but with more fields:

```json
[
  {
    "ein": 363673599,
	"name": 'Feed America'
    "exempt": 'y'
  }
]
```

#Forthcoming functionality
##We want this tool to be valuable to analysts who want to assess national expenditures in different charitable issue areas. In the future, you will be able to look up ein's for batch processing by various issue categorizations, as well as region. 

#Data
Data will be formated as a JSON, with the keys listed below. 
##zip

9-digit zip code for the organization

##irs_code_subsection

	Category under which an organization is exempt. Taken from the 501(c) of the Internal Revenue Code of 1986.
	
	[A list of subsection codes is available here:](https://www.irs.gov/pub/irs-soi/eo_info.pdf)


##classification_code_

There are four classification code variables, because each organization can have up to four listed. 0 is null. 

Classification codes are a sub-classification of IRS subsection codes. Note that the meaning of the categorical value depends on the organization's subsection code, so two groups with a classification code of 1, but with different subsection codes, do not fall into the same category. Up to four classification codes are listed. 0 means that there is no code listed.

[A list of classification codes](https://www.irs.gov/pub/irs-soi/eo_info.pdf), and their corresponding subsection codes, can be found [here](https://www.irs.gov/pub/irs-soi/eo_info.pdf). 


##affiliation_code

The affiliation code indicates an organization's membership in an geographical groupings, at the national, regional, or geographic level, and its ranking within that grouping's hierarchy.

Code | Description
---------- | -------
1 | Central organization, with no group exemption
2 | Intermediate organization. Not the most central organization, but not at the most local level of geographical aggregation
3 | Independent. Organization is not associated with a geographical grouping, or is an independent auxiliary
6 | Central organization,  that is a parent (group ruling), but is not a church or 501(c)(1) organization. 
7 | Exempt organization at an intermediate level of a geographical grouping of organizations. 
8 | Central organization that is a parent (group ruling) and is a church or 501(c)(1) organization
9 | Subordinate group in a group ruling
 
##ruling_date
	Year and month (YYYYMM) that the organization was ruled or determination letter recognizing the group as a non-profit entity.

##deductibility_code
Indicates whether contributions to the organization are tax deductible
Code | Description
---------- | -------
1|Contributions are deductible 
2|Contributions are not deductible
4|Contributions are deductible by treaty (foreign organizations)

##ntee
	Full NTEE code, used to classify exempt organizations in terms of their primary exempt activity. The first digit describes activities in support of nonprofit organizations. The fourth digit defines classification of the organization
	
	[Classifications can be found here:](https://www.irs.gov/pub/irs-soi/eo_info.pdf)

##ntee_category
Taken from the NTEE first letter, the ntee category is a broad categorization of the organization's primary activity. Examples include education, or health. 

[A full table of NTEE categories](https://www.irs.gov/pub/irs-soi/eo_info.pdf), with their associated NTEE sub-categories, is available [here.](https://www.irs.gov/pub/irs-soi/eo_info.pdf) 

##foundation_code_specific
Foundation codes are another categorization of organizations, relating to their tax exempt status. 


Code | Description
---------- | -------
00|All non-501(c)(3) organizations
02|Private operating foundations exempt from paying excise taxes on investment income
03|Private operating foundation (other)
04|Private non-operating foundation
09|Suspense
10|Church 170(b)(1)(i)
11|School 170(b)(1)(ii)
12|Hospital or other medical research organization 170(b)(1)(iii)
13|Organization which operates for beneit of college or university and is owned or operated by a government unit 170(b)(1)(iv)
14|Governmental unit 170(b)(1)(v)
15|Organization which recieves a substantial part of its support from a governmental unit or general public 170(b)(1)(vi)
16|Organization that normally receives no more than one-third of its support from gross investment income and unrelated business income and at the same time more than one-third of its support from contributions, fees, and gross receipts related to exempt purposes. 509(a)(2) 
17|Organizations operated solely for the benefit of and in conjunction with organizations described with foundation codes 10-16, 509(a)(3)
18|Organizations organized and operated to test for public safety 509(a)(4)
21|509(a)(3) Type I
22|509(a)(3) Type II
23|509(a)(3) Type III functionally integrated
24|509(a)(3) Type III not functionally integrated

##activity_code
Code reflecting the organization's purpose, activities, operations, or type. Note that this code was replaced by the NTEE, but exists for groups existing prior to 1995. Wher it exists, it has not been updated since 1994.

[A full table of activity codes](https://www.irs.gov/pub/irs-soi/eo_info.pdf) is available [here.](https://www.irs.gov/pub/irs-soi/eo_info.pdf) 

##organization_code
The organization type code indicates the type of organization

Code | Description
---------- | -------
1|Corporation
2|Trust
3|Co-operative
4|Partnership
5|Association

##last_return
The tax period corresponding with the latest return filed. (YYYYMM)

##accounting_end
The accounting period end is the last month of the organization's reporting period/fiscal year

##filing_requirement
The filing requirement indicates the the documents that the organization is required to file. 

Code | Description
---------- | -------
01 |990 (all other) or 990EZ return
02 |990 - Required to file Form 990-N - Income less than $25,000 per year
03 |990 - Group return
04 |990 - Required to file Form 990-BL, Black Lung Trusts
06 |990 - Not required to file (church)
07 |990 - Government 501(c)(1)
13 |990 - Not required to file (religious organization)
14 |990 - Not required to file (instrumentalities of states or political subdivisions)
00 |990 - Not required to file (all other) 

##pf_return_requirement
This is a boolean indicating whether the organization is required to fill out a 990-PF return.

##asset_code
The asset code is a categorical indicator of the shown on the organization's most recent Form 990 series return

Code | Description
---------- | -------
0 |$0
1 |$1 to $9,999
2 |$10,000 to $24,999
3 |$25,000 to $99,999
4 |$100,000 to $499,999
5 |$500,000 to $999,999
6 |$1,000,000 to $4,999,999
7 |$5,000,000 to $9,999,999
8 |$10,000,000 to $49,999,999
9 |$50,000,000 or more 

##asset_amount
The dollar amount of assets reported on the organization's latest 990 series return

##income_code
	Categorical indicator of an organization's income, according to their most recent Form 990 series return. Amount of income, generated by adding the elements of form 990 Part VIII or 990EZ Part 1. 
	
	Specifically: Income is generated using:

	Part VIII, Line 1h(A), Contributions/Gifts + Part VIII Line 2g(A), Program Revenue + Part VIII Line 3(A), Investment Income + Party VIII Line 4(A) Tax Exempt Bond Proceeds + Part VIII, Line 5(A), Royalties income + Part VIII line 6d(A), Rental Income + Party VIII, Line 7d(A), Net Gain/Loss amount + Part VIII Line 8c(A), Net fundraising + Part VIII, Line 9c(A), Net Gaming + Part VIII, Line 10c(A), Net Inventory sales + part VIII Line 11e(A), Miscelaneous Revenue. 
	
	On Form 990-EZ, Income Amount is a computer generated amount using:

	Part I Line 1, Total Contributions/Gifts +  Part I Line 2, Program Services amount + Part I Line 2, Dues and Assessments + Part I Line 4, Investment Income + Part I Line 5c, Sale of Assets + Part I Line 6c, Net Income fundraising + Part I Line 7c, Gross Profit + Part I Line 8, Other Revenue.

	The categorical breakdown is:
	
	Code | Description
---------- | -------
0 |$0
1 |$1 to $9,999
2 |$10,000 to $24,999
3 |$25,000 to $99,999
4 |$100,000 to $499,999
5 |$500,000 to $999,999
6 |$1,000,000 to $4,999,999
7 |$5,000,000 to $9,999,999
8 |$10,000,000 to $49,999,999
9 |$50,000,000 or more 

##income_amount
Amount of income, generated by adding the elements of form 990 Part VIII or 990EZ Part 1. Specifically, Income is generated using:

 Part VIII, Line 1h(A), Contributions/Gifts + Part VIII Line 2g(A), Program Revenue + Part VIII Line 3(A), Investment Income + Party VIII Line 4(A) Tax Exempt Bond Proceeds + Part VIII, Line 5(A), Royalties income + Part VIII line 6d(A), Rental Income + Party VIII, Line 7d(A), Net Gain/Loss amount + Part VIII Line 8c(A), Net fundraising + Part VIII, Line 9c(A), Net Gaming + Part VIII, Line 10c(A), Net Inventory sales + part VIII Line 11e(A), Miscelaneous Revenue. 

On Form 990-EZ, Income Amount is a computer generated amount using:

 Part I Line 1, Total Contributions/Gifts +  Part I Line 2, Program Services amount + Part I Line 2, Dues and Assessments + Part I Line 4, Investment Income + Part I Line 5c, Sale of Assets + Part I Line 6c, Net Income fundraising + Part I Line 7c, Gross Profit + Part I Line 8, Other Revenue.

##revenue_990
Revenue reported in form 990, Part VIII Line 12(A), or 990-EZ Part I Line 9. 

##tax_year
The tax year of the most recent return

##organization_name
Name under which the organization files

##doing_business_as
Organizations can specify up to three aliases

##gross_receipts_under_25k
Boolean, did the organization receive less than $25,000 during the most recent reporting period

##website_url
The organization's website_url

##terminated
Boolean, indicating whether the group has been terminated

##officer_X
Name and address of an officer from the organizationThese include:

* name
* address_line_1
* address_line 2
* address_city
* address_province
* address_state_id
* address_postal_code
* address_country

##organization_address_X
Address information for the organization. These include:

* line_1
* line_2
* city
* province
* state_id
* postal_code
* country


##legal_name
Organization's legal name

##city
City where the organization is headquarted

##state_id
State where the organization is headquartered

##country
Country where the organization is headquartered

##status
Categorical indicator of whether donations are tax deductible 
Code | Description
---------- | -------
1|Contributions are deductible 
2|Contributions are not deductible
4|Contributions are deductible by treaty (foreign organizations)

##revocation_date
The date the organization's status was automatically revoked because of failure to fill out a return

##revocation_posting_date
Date that the group's revocation was posted. 
