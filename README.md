# Uilicious Somewhat Unofficial API guide

The following is the not-so-official API guide - what do I mean by that? We are not committing to zero api breakages and changes. Also I am not maintaining this to the level of what would be considered "industry standard".

# First get you ACCESS_KEY

Login to UI-licious Studio, go to "Account and Billing > Access Key" to view and regenerate your access key

You can use the access key as a GET/POST parameter for any request. A
Alternatively you can send it as a header as `accesskey`.

For example, you can do either of the following

```
# accesskey as a GET parameter
curl --location --request GET "https://api.uilicious.com/v3.0/project/testrun/list/running?projectID=$PROJECT_ID&accesskey=<accessKey>" 

# accesskey as a Header
curl --location --header "accesskey: $ACCESS_KEY" --request GET "https://api.uilicious.com/v3.0/project/testrun/list/running?projectID=$PROJECT_ID" 
```

All API responses are in JSON unless stated otherwise

# API commands for ...

### Getting test results

`https://api.uilicious.com/v3.0/project/testrun/get?id=<testID>`

Example response

```
{
	"result" : {
		"type" : "MANUAL",
    "id" : "8JWGHaqm1ovYYvR2G54oam",
		"testRunBillID" : "EAMdDc1hSCsQJfSPoDpMi6",
		"createdAt" : 1647175995,
		"browser" : "chrome",
		"status" : "pending"
		"resolution" : "1280x960",
		...
		"result" : {
			"id" : "8JWGHaqm1ovYYvR2G54oam",
			"steps" : [
				{ ... }
			]
		}
	}
}
```

Full example of result can be found here : https://gist.github.com/PicoCreator/99897d11e4645b61c1216d8eec20daf2

### Getting current test concurrency

`https://api.uilicious.com/v3.0/project/testrun/list/running?projectID=<projectID>`

Example response

```
{
  "result": [{ .. details of a running test .. }],
  "concurrency": 2
}
```
