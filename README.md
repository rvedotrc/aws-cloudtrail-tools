Fetching
--------

Fetch some CloudTrail logs to local storage:

`./bin/pull-cloudtrail-logs [--dry-run] BUCKET ACCOUNTID REGION STARTTIME ENDTIME`

(Requires AWS credentials to read the given BUCKET).  There is no support yet
for the "bucket prefix" option – the logs are assumed to have object keys like
`AWSLogs/123456789012/CloudTrail/eu-west-1/2016/03/25/123456789012_CloudTrail_eu-west-1_20160325T2230Z_HUERLu3a4rXsZcV0.json.gz`.

STARTTIME and ENDTIME can be in various formats.  The tool tries its best to
make sense of whatever argument you give it.

The actual downloading is currently done using the (python-based) `aws s3 cp`
command.

Dealing with duplicate logs
---------------------------

Especially on quiet accounts, duplicate logs can be delivered, so use
`unique-logs` to eliminate the duplicates.  Its argument (or arguments) is a
filename glob, expanded internally.  This allows you easily to avoid shell glob
expansion limits.

For example to create a single (multi-object) JSON file containing all of the
logs together:

`./bin/unique-logs 'AWSLogs/*/CloudTrail/*/*/*/*/*.json.gz' | xargs gunzip -c > all.json`

Analysing
---------

`cat all.json | jq -c .Records[] | ./bin/analyse | less`

