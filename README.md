`./bin/unique-logs 'AWSLogs/[0-9]*/CloudTrail/*/*/*/*/*.json.gz' | xargs gunzip -c > all.json`

`cat all.json | jq -c .Records[] | ./bin/analyse | -`

