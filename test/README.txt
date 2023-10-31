vespa version should be 7.594.36

Deployment:

from the root folder of the configs:

# Setup
$ tar -czf - . | curl --header Content-type:application/x-gzip --data-binary @- http://<vespa-host>:19071/application/v2/tenant/default/prepareandactivate

# Get current active session id
curl http://<vespa-host>:19071/application/v2/tenant/default/application/default

# Read the schema file for verification that we're all set (use the session id from the previous call)
curl http://<vespa-host>:19071/application/v2/tenant/default/session/<current-active-session-id>/content/schemas/call.sd


# To perform a query, fire a POST call to http://<vespa-host>:8080/search with the following payload:
{
	"yql": "select * from call where true",
	"ranking.features.query(query_vector)": [<vector here>, list of floats], 
	"ranking.profile": "cosine-similarity"
}

# To filter by start_time
where range(start_time, unix_timestamp_from, unix_timestamp_to)