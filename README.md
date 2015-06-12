
## Getting the raw logs from S3

_Note_: log files I've pulled down already are in `logs/` if you want to skip this step.


You'll need the Amazon Web Services credentials with access to the bucket entered as environmental variables (or `credentials` file):

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION`

Now either install the Amazon Web Services command line tool (`awscli`) or drop into a docker container with AWS installed.


```
docker run --rm -ti -v $(pwd):/data -w /data \
  -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
  -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
  -e AWS_DEFAULT_REGION=$AWS_DEFAULT_REGION \
  cboettig/aws

```

Copy the log files into the local `logs` directory:

```
aws s3 cp --recursive s3://drat/logs logs
```

Great, you now have the log files.


## Parsing logs with python

_Note_: I've parsed the first several logs into `log.csv` already if you want to start there.

Install the python dependencies (`pandas`, `dateutil`) or drop into the docker container as above. Then run the `parses3logs.py` script (from the repository root directory) to generate a parsed version of the logs, `log.csv`. 

## Processing the logs

Now one needs to filter out all the activity that involves the `aws-internal` or `S3-Console` user agent, etc., to isolate which events are actual downloads. More tricks may be necessary to handle incomplete downloads etc. 

