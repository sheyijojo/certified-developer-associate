## Question 

You are creating a Cloud Formation template to deploy your CMS application running on an EC2 instance within your AWS account. 

Since the application will be deployed across multiple regions, you need to create a map of all the possible values for the base AMI.

How will you invoke the !FindInMap function to fulfill this use case?

```yaml
!FindInMap [ MapName, TopLevelKey ]

!FindInMap [ MapName, TopLevelKey, SecondLevelKey, ThirdLevelKey ]

Your answer is incorrect
!FindInMap [ MapName ]

Correct answer
!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]

```

## Notes 
```yaml
Overall explanation
Correct option:

!FindInMap [ MapName, TopLevelKey, SecondLevelKey ] - The intrinsic function Fn::FindInMap returns the value corresponding to keys in a two-level map that is declared in the Mappings section. YAML Syntax for the full function name: Fn::FindInMap: [ MapName, TopLevelKey, SecondLevelKey ]

Short form of the above syntax is : !FindInMap [ MapName, TopLevelKey, SecondLevelKey ]

Where,

MapName - Is the logical name of a mapping declared in the Mappings section that contains the keys and values. TopLevelKey - The top-level key name. Its value is a list of key-value pairs. SecondLevelKey - The second-level key name, which is set to one of the keys from the list assigned to TopLevelKey.

//Consider the following YAML template:
```

```yaml
Mappings:
  RegionMap:
    us-east-1:
      HVM64: "ami-0ff8a91507f77f867"
      HVMG2: "ami-0a584ac55a7631c0c"
    us-west-1:
      HVM64: "ami-0bdb828fd58c52235"
      HVMG2: "ami-066ee5fd4a9ef77f1"
    eu-west-1:
      HVM64: "ami-047bb4163c506cd98"
      HVMG2: "ami-31c2f645"
    ap-southeast-1:
      HVM64: "ami-08569b978cc4dfa10"
      HVMG2: "ami-0be9df32ae9f92309"
    ap-northeast-1:
      HVM64: "ami-06cd52961ce9f0d85"
      HVMG2: "ami-053cdd503598e4a9d"
Resources:
  myEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: !FindInMap
        - RegionMap
        - !Ref 'AWS::Region'
        - HVM64
      InstanceType: m1.small
```

## Explanation 
```md
The example template contains an AWS::EC2::Instance resource whose ImageId property is set by the FindInMap function.

MapName is set to the map of interest, "RegionMap" in this example. TopLevelKey is set to the region where the stack is created, which is determined by using the "AWS::Region" pseudo parameter. SecondLevelKey is set to the desired architecture, "HVM64" for this example.

FindInMap returns the AMI assigned to FindInMap. For a HVM64 instance in us-east-1, FindInMap would return "ami-0ff8a91507f77f867".

```

Domain - Development with AWS 

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-findinmap.html

```md
Overall explanation
Correct options:

Use an error retry and exponential backoff mechanism

Decrease the frequency or size of your requests

You can use PutRecords API call to write multiple data records into a Kinesis data stream in a single call. Each PutRecords request can support up to 500 records. Each record in the request can be as large as 1 MiB, up to a limit of 5 MiB for the entire request, including partition keys. Each shard can support writes up to 1,000 records per second, up to a maximum data write of 1 MiB per second.

The response Records array includes both successfully and unsuccessfully processed records. Kinesis Data Streams attempts to process all records in each PutRecords request. A single record failure does not stop the processing of subsequent records. As a result, PutRecords doesn't guarantee the ordering of records. An unsuccessfully processed record includes ErrorCode and ErrorMessage values. ErrorCode reflects the type of error and can be one of the following values: ProvisionedThroughputExceededException or InternalFailure. ProvisionedThroughputExceededException indicates that the request rate for the stream is too high, or the requested data is too large for the available throughput. Reduce the frequency or size of your requests.

To address the given use case, you can apply these best practices:

Reshard your stream to increase the number of shards in the stream.

Reduce the frequency or size of your requests.

Distribute read and write operations as evenly as possible across all of the shards in Data Streams.

Use an error retry and exponential backoff mechanism.

```
