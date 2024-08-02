Domain - Troubleshooting and optimization 

```md
A data analytics company processes Internet-of-Things (IoT) data using Amazon Kinesis. The development team has noticed that the IoT data feed into Kinesis experiences periodic spikes. The PutRecords API call occasionally fails and the logs show that the failed call returns the response shown below:


```
x-amzn-RequestId: The x-amzn-RequestId header typically represents a unique identifier for a request made to an AWS service. This identifier can be useful for tracking and troubleshooting individual request
```sh
HTTP/1.1 200 OK
x-amzn-RequestId: <RequestId>
Content-Type: application/x-amz-json-1.1
Content-Length: <PayloadSizeBytes>
Date: <Date>
{
    "FailedRecordCount": 2,
    "Records": [
        {
            "SequenceNumber": "49543463076548007577105092703039560359975228518395012686",
            "ShardId": "shardId-000000000000"
        },
        {
            "ErrorCode": "ProvisionedThroughputExceededException",
            "ErrorMessage": "Rate exceeded for shard shardId-000000000001 in stream exampleStreamName under account 111111111111."
        },
        {
            "ErrorCode": "InternalFailure",
            "ErrorMessage": "Internal service failure."
        }
    ]
}


As an AWS Certified Developer Associate, which of the following options would you recommend to address this use case? (Select two)

Decrease the number of KCL consumers

Your selection is correct
Use an error retry and exponential backoff mechanism

Correct selection
Decrease the frequency or size of your requests

Your selection is incorrect
Increase the frequency or size of your requests

Merge the shards to decrease the number of shards in the stream

```

## Notes 
References:

https://aws.amazon.com/premiumsupport/knowledge-center/kinesis-readprovisionedthroughputexceeded/

https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html
```md

## Overall explanation
Correct options:

Use an error retry and exponential backoff mechanism

Decrease the frequency or size of your requests

You can use PutRecords API call to write multiple data records into a Kinesis data stream in a single call. Each PutRecords request can support up to 500 records.


Each record in the request can be as large as 1 MiB, up to a limit of 5 MiB for the entire request, including partition keys. Each shard can support writes up to 1,000 records per second, up to a maximum data write of 1 MiB per second.

The response Records array includes both successfully and unsuccessfully processed records. Kinesis Data Streams attempts to process all records in each PutRecords request. A single record failure does not stop the processing of subsequent records. As a result, PutRecords doesn't guarantee the ordering of records. 


An unsuccessfully processed record includes ErrorCode and ErrorMessage values. ErrorCode reflects the type of error and can be one of the following values: ProvisionedThroughputExceededException or InternalFailure.


ProvisionedThroughputExceededException indicates that the request rate for the stream is too high, or the requested data is too large for the available throughput. Reduce the frequency or size of your requests.

To address the given use case, you can apply these best practices:

Reshard your stream to increase the number of shards in the stream.

Reduce the frequency or size of your requests.

Distribute read and write operations as evenly as possible across all of the shards in Data Streams.

Use an error retry and exponential backoff mechanism.

```