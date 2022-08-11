# Uilicious Somewhat Unofficial API guide

The following is the not-so-official API guide - what do I mean by that? 

I am not maintaining this to the level of what would be considered "industry standard".
We are not committing to zero api breakages and changes (though we avoid doing so as much as possible, because updating all the UI calls is a hassle).

 - so YMMV, and use at your own risk =X

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

### Running a test in a project

You can trigger a test using the following API with a POST request

`https://api.uilicious.com/v3.0/project/testrun/start`

You will be required to send the following either as a `application/x-www-form-urlencoded` or `application/json` format

```
{
	"projectID": "<your project ID>",
	"browser": "chrome",
	"serverRegion": "default",
	"runFile": "<filename of test to run in your project>"
}
```

You would get the following if the test was started succesfully

```
{
	"result" : {
		"testRunIDs" : [
			"<you test ID>"
		],
		"testRunBillID" : "<internal request ID>",
		"testRuns" : [
			{ ... additional information on your test run ... }
		]
	}
}
```

### Getting test results

> Note: test results do expire after 3 months 

`https://api.uilicious.com/v3.0/project/testrun/get?id=<test ID>`

Example response

```
{
	"result" : {
		"type" : "MANUAL",
		"id" : "<test ID>",
		"testRunBillID" : "<internal request ID>",
		"createdAt" : 1647175995,
		"browser" : "chrome",
		"status" : "pending"
		"resolution" : "1280x960",
		...
		"result" : {
			"id" : "<test ID>",
			"steps" : [
				{ ... }
			]
		}
	}
}
```

Full example of result can be found here : https://gist.github.com/PicoCreator/99897d11e4645b61c1216d8eec20daf2

### Running a JOB in a project

You can trigger a scheduled JOB using the following API with a POST request

`https://api.uilicious.com/v3.0/project/job/<jobID>/run`

You would get the following if the test was started succesfully

```
{
	"result" : {
		"testRunIDs" : [
			"<you test ID>"
		],
		"testRunBillID" : "<internal request ID>",
		"testRuns" : [
			{ ... additional information on your test run ... }
		]
	}
}
```

You will get the following response

```
{
	"result" : {
		.... (note most values here are internal ID and is not useful directly) ....
		"status" : "created"
	}
}
```

Note you will get the following if the JOB is already scheduled / running

```
{
	"ERROR" : {
		"stack" : "...",
		"code" : "ALREADY_RUNNING_JOB",
		"message" : "There's already a pending task to run this job."
	}
}
```

### Getting the latest JOB result

At any point of time, you can get the latest status of a test job using

`https://api.uilicious.com/v3.0/project/job/testrunset/list?start=0&length=1&jobID=<job ID>`

This should give the following response

```
{
	"result" : [
		{
			// .... (note most values here are internal ID and is not useful directly) ....

			"testRunIDs" : [
				"< test run ID, one for each browser >"
			],
			"jobID" : "< job ID >",
			"testRunRequests" : [
				{
					"testRunIDs" : [
						"< test run ID >""
					],
					
					// ... (additional information on your test run) ...

					// Status of a test
					"status" : "failure"
				},

				// ... additional objects for each browser in job ...
			],
			"testRuns" : [
				{
					"id" : "< test run ID >"",
					
					// ... (additional information on your test run) ...

					// Status of a test
					"status" : "failure"
				},

				// ... additional objects for each browser in job ...
			],
			
			"runTime" : 1660188429,
			"projectID" : "< project ID >",

			// Overall status of the job
			"status" : "failure"
		}
	],
	"start" : 0,
	"length" : 1,
	"totalCount" : 2
}
```

### Getting current test concurrency

`https://api.uilicious.com/v3.0/project/testrun/list/running?projectID=<projectID>`

Example response

```
{
  "result": [{ .. details of a running test .. }],
  "concurrency": 2
}
```
