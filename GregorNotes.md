Cloud9 default is Python 3.7. 3.9 only needed because of ":=" syntactic sugar

## EventBridge

arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local
arn:aws:events:ap-southeast-1:976509018599:rule/UnicornPropertiesEventBus-Local/properties.catchall

Trying to use input transformer:
Input for target PropertiesServiceCatchAllLogGroupTarget-Local is not a valid JSON text.
Input and InputPath are not valid forms of input for target Target0. The only valid form of input is InputTransformer.

Event in DLQ:
{
    "version": "0",
    "id": "4fd2a95d-0e26-34a0-6ac0-4b7ce1d06935",
    "detail-type": "ContractStatusChanged",
    "source": "unicorn.contracts",
    "account": "976509018599",
    "time": "2023-10-02T10:04:13Z",
    "region": "ap-southeast-1",
    "resources": [],
    "detail": {
        "contract_last_modified_on": "10/02/2023 10:04:13",
        "property_id": "usa/anytown/main-street/181",
        "contract_id": "988d110e-6da7-4f14-aef0-f6a28443dd49",
        "contract_status": "DRAFT"
    }
}

Event from Pipes
{
    "version": "0",
    "id": "7c63f949-0493-c317-5cc8-dcf8304b61b0",
    "detail-type": "ContractStatusChanged-Pipes",
    "source": "unicorn.contracts",
    "account": "976509018599",
    "time": "2023-02-10T10:04:14Z",
    "region": "ap-southeast-1",
    "resources": [],
    "detail": {
        "contract_last_modified_on": "10/02/2023 10:04:13",
        "property_id": "usa/anytown/main-street/181",
        "contract_id": "988d110e-6da7-4f14-aef0-f6a28443dd49",
        "contract_status": "DRAFT"
    }
}

Response from put_events:
{
    "FailedEntryCount": 0,
    "Entries": [
        {
            "EventId": "4fd2a95d-0e26-34a0-6ac0-4b7ce1d06935"
        }
    ],
    "ResponseMetadata": {
        "RequestId": "6990c480-9928-43c9-a761-9444de7dc89f",
        "HTTPStatusCode": 200,
        "HTTPHeaders": {
            "x-amzn-requestid": "6990c480-9928-43c9-a761-9444de7dc89f",
            "content-type": "application/x-amz-json-1.1",
            "content-length": "85",
            "date": "Fri,10 Feb 2023 10: 04: 13 GMT"
        },
        "RetryAttempts": 0
    }
}

## Pipes Source and Detail

Pipes arn: arn:aws:pipes:ap-southeast-1:976509018599:pipe/Contracts

Event from Lambda:

{
  "Time": "08/02/2023 16: 06: 56",
  "Source": "unicorn.contracts",
  "DetailType": "ContractStatusChanged",
  "Detail": {
      "contract_last_modified_on": "08/02/2023 16:06:56",
      "property_id": "usa/anytown/main-street/153",
      "contract_id": "25a238b6-3143-41e2-a7b9-d2b66be4bb03",
      "contract_status": "DRAFT"
  },
  "EventBusName": "UnicornPropertiesEventBus-Local"
}

Event from Pipes
{
    "version": "0",
    "id": "49ea11e1-41ee-fd51-545a-d2ae6f203195",
    "detail-type": "Event from null",
    "source": "Pipe Contracts",
    "account": "976509018599",
    "time": "2023-02-06T09:06:32Z",
    "region": "ap-southeast-1",
    "resources": [],
    "detail": {
        "contract_last_modified_on": "06/02/2023 09:06:31",
        "property_id": "usa/anytown/main-street/150",
        "contract_id": "c66d4ffc-27e6-42d2-b635-121eee31cda3",
        "contract_status": "DRAFT"
    }
}

{
    "version": "0",
    "id": "89961743-7396-b27b-4874-50949f07c9bb",
    "detail-type": "Event from null",
    "source": "Pipe Contracts",
    "account": "976509018599",
    "time": "2023-02-08T15:41:01Z",
    "region": "ap-southeast-1",
    "resources": [],
    "detail": {
        "eventID": "faae270c2d33f1556745b22178c47f4a",
        "eventName": "INSERT",
        "eventVersion": "1.1",
        "eventSource": "aws:dynamodb",
        "awsRegion": "ap-southeast-1",
        "dynamodb": {
            "ApproximateCreationDateTime": 1675870861,
            "Keys": {
                "property_id": {
                    "S": "usa/anytown/main-street/152"
                }
            },
            "NewImage": {
                "contract_last_modified_on": {
                    "S": "08/02/2023 15:41:00"
                },
                "address": {
                    "M": {
                        "country": {
                            "S": "USA"
                        },
                        "number": {
                            "N": "152"
                        },
                        "city": {
                            "S": "Anytown"
                        },
                        "street": {
                            "S": "Main Street"
                        }
                    }
                },
                "seller_name": {
                    "S": "John Smith"
                },
                "contact_created": {
                    "S": "08/02/2023 15:41:00"
                },
                "contract_id": {
                    "S": "d4745ecb-ac96-4764-ba88-6d2761e96e88"
                },
                "contract_status": {
                    "S": "DRAFT"
                },
                "property_id": {
                    "S": "usa/anytown/main-street/152"
                }
            },
            "SequenceNumber": "46534700000000000643392612",
            "SizeBytes": 303,
            "StreamViewType": "NEW_IMAGE"
        },
        "eventSourceARN": "arn:aws:dynamodb:ap-southeast-1:976509018599:table/uni-prop-local-contract-ContractsTable-10X09BGH3CXR2/stream/2023-02-01T15:09:35.555"
    }
}

PipeTargetEventBridgeEventBusParameters
https://docs.aws.amazon.com/eventbridge/latest/pipes-reference/API_PipeTargetEventBridgeEventBusParameters.html

## Pipes Transform

```
{
  "contract_last_modified_on": <$.dynamodb.NewImage.contract_last_modified_on.S>,
  "property_id": <$.dynamodb.NewImage.property_id.S>,
  "contract_id": <$.dynamodb.NewImage.contract_id.S>,
  "contract_status": <$.dynamodb.NewImage.contract_status.S>
}
```

{
    "version": "0",
    "id": "7ba242f7-5a2c-81d3-7e08-2837996cb2d3",
    "detail-type": "Event from null",
    "source": "Pipe Contracts",
    "account": "976509018599",
    "time": "2023-02-08T16:06:56Z",
    "region": "ap-southeast-1",
    "resources": [],
    "detail": {
        "contract_last_modified_on": "08/02/2023 16:06:56",
        "property_id": "usa/anytown/main-street/153",
        "contract_id": "25a238b6-3143-41e2-a7b9-d2b66be4bb03",
        "contract_status": "DRAFT"
    }
}

## Pipes Describe

aws pipes describe-pipe --name=Contracts
{
    "Arn": "arn:aws:pipes:ap-southeast-1:976509018599:pipe/Contracts",
    "CreationTime": "2023-02-01T15:11:37+00:00",
    "CurrentState": "RUNNING",
    "Description": "Send contract updates from DDB to EB",
    "DesiredState": "RUNNING",
    "EnrichmentParameters": {},
    "LastModifiedTime": "2023-02-08T16:06:53+00:00",
    "Name": "Contracts",
    "RoleArn": "arn:aws:iam::976509018599:role/service-role/Amazon_EventBridge_Pipe_Contracts_9f0bbeeb",
    "Source": "arn:aws:dynamodb:ap-southeast-1:976509018599:table/uni-prop-local-contract-ContractsTable-10X09BGH3CXR2/stream/2023-02-01T15:09:35.555",
    "SourceParameters": {
        "DynamoDBStreamParameters": {
            "BatchSize": 1,
            "StartingPosition": "LATEST"
        }
    },
    "StateReason": "No records processed",
    "Tags": {},
    "Target": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local",
    "TargetParameters": {
        "InputTemplate": "{\n  \"contract_last_modified_on\": <$.dynamodb.NewImage.contract_last_modified_on.S>,\n  \"property_id\": <$.dynamodb.NewImage.property_id.S>,\n  \"contract_id\": <$.dynamodb.NewImage.contract_id.S>,\n  \"contract_status\": <$.dynamodb.NewImage.contract_status.S>\n}"
    }
}

aws pipes update-pipe --name="Contracts" --role-arn=arn:aws:iam::976509018599:role/service-role/Amazon_EventBridge_Pipe_Contracts_9f0bbeeb --target-parameters EventBridgeEventBusParameters="{DetailType=ContractStatusChanged,Source=unicorn.contracts}"

aws pipes describe-pipe --name=Contracts              {
    "Arn": "arn:aws:pipes:ap-southeast-1:976509018599:pipe/Contracts",
    "CreationTime": "2023-02-01T15:11:37+00:00",
    "CurrentState": "RUNNING",
    "Description": "Send contract updates from DDB to EB",
    "DesiredState": "RUNNING",
    "EnrichmentParameters": {},
    "LastModifiedTime": "2023-02-08T16:39:59+00:00",
    "Name": "Contracts",
    "RoleArn": "arn:aws:iam::976509018599:role/service-role/Amazon_EventBridge_Pipe_Contracts_9f0bbeeb",
    "Source": "arn:aws:dynamodb:ap-southeast-1:976509018599:table/uni-prop-local-contract-ContractsTable-10X09BGH3CXR2/stream/2023-02-01T15:09:35.555",
    "SourceParameters": {
        "DynamoDBStreamParameters": {
            "BatchSize": 1,
            "StartingPosition": "LATEST"
        }
    },
    "StateReason": "No records processed",
    "Tags": {},
    "Target": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local",
    "TargetParameters": {
        "EventBridgeEventBusParameters": {
            "DetailType": "ContractStatusChanged",
            "Source": "unicorn.contracts"
        },
        "InputTemplate": "{\n  \"contract_last_modified_on\": <$.dynamodb.NewImage.contract_last_modified_on.S>,\n  \"property_id\": <$.dynamodb.NewImage.property_id.S>,\n  \"contract_id\": <$.dynamodb.NewImage.contract_id.S>,\n  \"contract_status\": <$.dynamodb.NewImage.contract_status.S>\n}"
    }
}

## Lambda Log

[{'Time': '09/02/2023 03:12:51', 'Source': 'unicorn.contracts', 'DetailType': 'ContractStatusChanged', 'Detail': '{"contract_last_modified_on": "09/02/2023 03:12:51", "property_id": "usa/anytown/main-street/174", "contract_id": "fb759ce7-aad7-47bd-afa8-60241f0f790a", "contract_status": "DRAFT"}', 'EventBusName': 'UnicornPropertiesEventBus-Local'}]

{'FailedEntryCount': 0, 'Entries': [{'EventId': '8291a24b-872b-fc1f-3c82-66b67ba36f81'}], 'ResponseMetadata': {'RequestId': '0a6799ab-265e-45cc-8919-3d723261a699', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amzn-requestid': '0a6799ab-265e-45cc-8919-3d723261a699', 'content-type': 'application/x-amz-json-1.1', 'content-length': '85', 'date': 'Thu, 09 Feb 2023 03:12:51 GMT'}, 'RetryAttempts': 0}}

## Permissions Issues
aws events put-permission --event-bus-name=UnicornPropertiesEventBus-Local --principal=976509018599 --action=events:PutEvents --statement-id=PutForAll 

aws logs put-resource-policy --policy-name AllowLogEvent --policy-document '{ "Version": "2012-10-17", "Statement": [ { "Sid": "EventBridgeToCloudWatchLogs", "Effect": "Allow", "Principal": { "Service": [ "events.amazonaws.com" ] }, "Action": "logs:PutLogEvents", "Resource": "arn:aws:logs:*:976509018599:log-group:*" } ] }'

aws logs put-resource-policy --policy-name AllowLogEvent --policy-document '{ "Version": "2012-10-17", "Statement": [ { "Sid": "EventBridgeToCloudWatchLogs", "Effect": "Allow", "Principal": { "Service": [ "events.amazonaws.com","delivery.logs.amazonaws.com" ] }, "Action": ["logs:PutLogEvents", "logs:CreateLogStream"], "Resource": "arn:aws:logs:*:976509018599:log-group:*:*" } ] }' 

aws events describe-event-bus --name=UnicornPropertiesEventBus-Local --output=json --query=Policy

```
{
    "Statement": [
      {
        "Action": [
          "logs:CreateLogStream",
          "logs:PutLogEvents"
        ],
        "Effect": "Allow",
        "Principal": {
          "Service": [
            "events.amazonaws.com",
            "delivery.logs.amazonaws.com"
          ]
        },
        "Resource": "arn:aws:logs:ap-southeast1::log-group:/aws/events/*:*",
        "Sid": "TrustEventsToStoreLogEvent"
      }
    ],
    "Version": "2012-10-17"
  }

  [
    {
        "Sid": "ContactsPublishEventsPolicy-Local",
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::976509018599:root"
        },
        "Action": "events:PutEvents",
        "Resource": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local"
    },
    {
        "Sid": "PutForAll",
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::976509018599:root"
        },
        "Action": "events:PutEvents",
        "Resource": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local"
    },
    {
        "Sid": "CreateRulePolicy-Local",
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::976509018599:root"
        },
        "Action": [
            "events:PutRule",
            "events:DeleteRule",
            "events:DescribeRule",
            "events:DisableRule",
            "events:EnableRule",
            "events:PutTargets",
            "events:RemoveTargets"
        ],
        "Resource": "arn:aws:events:ap-southeast-1:976509018599:rule/UnicornPropertiesEventBus-Local/*",
        "Condition": {
            "StringEquals": {
                "events:source": [
                    "unicorn.contracts",
                    "unicorn.properties"
                ]
            },
            "Null": {
                "events:source": "false"
            },
            "StringEqualsIfExists": {
                "events:creatorAccount": "${aws:PrincipalAccount}"
            }
        }
    }
]
  ```


```
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ContactsPublishEventsPolicy-Local",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::976509018599:root"
            },
            "Action": "events:PutEvents",
            "Resource": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local"
        },
        {
            "Sid": "PutForAll",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::976509018599:root"
            },
            "Action": "events:PutEvents",
            "Resource": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local"
        },
        {
            "Sid": "CreateRulePolicy-Local",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::976509018599:root"
            },
            "Action": [
                "events:PutRule",
                "events:DeleteRule",
                "events:DescribeRule",
                "events:DisableRule",
                "events:EnableRule",
                "events:PutTargets",
                "events:RemoveTargets"
            ],
            "Resource": "arn:aws:events:ap-southeast-1:976509018599:rule/UnicornPropertiesEventBus-Local/*",
            "Condition": {
                "StringEquals": {
                    "events:source": [
                        "unicorn.contracts",
                        "unicorn.properties"
                    ]
                },
                "Null": {
                    "events:source": "false"
                },
                "StringEqualsIfExists": {
                    "events:creatorAccount": "${aws:PrincipalAccount}"
                }
            }
        }
    ]
}
```

Lambda Role:
{
    "Statement": [
        {
            "Action": "events:PutEvents",
            "Resource": "arn:aws:events:ap-southeast-1:976509018599:event-bus/UnicornPropertiesEventBus-Local",
            "Effect": "Allow"
        }
    ]
}
