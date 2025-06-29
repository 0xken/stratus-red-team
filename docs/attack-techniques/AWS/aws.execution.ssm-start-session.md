---
title: Usage of ssm:StartSession on multiple instances
---

# Usage of ssm:StartSession on multiple instances

 <span class="smallcaps w3-badge w3-orange w3-round w3-text-sand" title="This attack technique might be slow to warm up or detonate">slow</span> 
 <span class="smallcaps w3-badge w3-blue w3-round w3-text-white" title="This attack technique can be detonated multiple times">idempotent</span> 

Platform: AWS

## Mappings

- MITRE ATT&CK
    - Execution



## Description


Simulates an attacker utilizing AWS Systems Manager (SSM) StartSession to gain unauthorized interactive access to multiple EC2 instances.

<span style="font-variant: small-caps;">Warm-up</span>:

- Create multiple EC2 instances and a VPC (takes a few minutes).

<span style="font-variant: small-caps;">Detonation</span>: 

- Initiates a connection to the EC2 for a Session Manager session.

References:

- https://awstip.com/responding-to-an-attack-in-aws-9048a1a551ac (evidence of usage in the wild)
- https://hackingthe.cloud/aws/post_exploitation/run_shell_commands_on_ec2/#session-manager
- https://unit42.paloaltonetworks.com/cloud-lateral-movement-techniques/


## Instructions

```bash title="Detonate with Stratus Red Team"
stratus detonate aws.execution.ssm-start-session
```
## Detection


Identify, through CloudTrail's <code>StartSession</code> event, when a user is starting an interactive session to multiple EC2 instances. Sample event:

```
{
  "eventSource": "ssm.amazonaws.com",
  "eventName": "StartSession",
  "requestParameters": {
    "target": "i-123456"
  },
  "responseElements": {
        "sessionId": "...",
        "tokenValue": "Value hidden due to security reasons.",
        "streamUrl": "wss://ssmmessages.eu-west-1.amazonaws.com/v1/data-channel/..."
   },
}
```



## Detonation logs <span class="smallcaps w3-badge w3-light-green w3-round w3-text-sand">new!</span>

The following CloudTrail events are generated when this technique is detonated[^1]:


- `ssm:DescribeInstanceInformation`

- `ssm:StartSession`

- `ssm:TerminateSession`


??? "View raw detonation logs"

    ```json hl_lines="6 48 90 132 174 216 258 300 342 384 426 468 510 552 594 636 678 720 762 804 846 888 930 972 1007 1044 1079 1116 1151"

    [
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "ab04bb55-b6d5-492b-8697-9d11867c6c43",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "c98780a2-d6a4-4114-91b0-a28a2a0842b3",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "5ccb707e-ea1c-4ae5-acb1-2039ca8908ec",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:15Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "089ef7a1-3dd7-4b8c-a59d-d169df9b4316",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "75d83a2a-99a3-4808-ade4-fe692446096b",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:14Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "9d1129f2-f619-4690-bab2-b097875b913f",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "9a3b3ce3-c139-46e2-be9b-920f6c670c42",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:12Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "45eb28df-eda5-4b72-8e11-3b37679681a0",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "b8a73842-fae3-40a9-85b3-515a1a07d582",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:11Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "cb435a50-9023-4ded-a904-6f448738ee31",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "554070cc-5bc1-4894-9880-c75a15ac78a2",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:10Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "8eb080f2-3c5d-447c-bad2-d4ceebe8bfd2",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "6844ea57-f22c-42e1-ae5b-709d8fc2c36b",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:09Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "84c1b5d3-c365-469c-917b-cc317aed7d43",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "913f3327-0ef4-4acb-a3a2-325ddcbda947",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:08Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "1b58a0d1-b841-4234-ad41-25faee08b985",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "b045bced-b93a-4e6c-a1b8-2011fe92b93a",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:06Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "ab3f6858-2db0-413f-9b21-09997a048505",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "0f520fea-16a0-459f-bf72-21efd8457cb1",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:05Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "6ca20f16-71aa-4794-8884-36989a3b7bc6",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "9546b899-0954-4c25-bbfb-a588f2a072c6",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:04Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "ec65f81b-3145-4abd-a992-1de519835cad",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "4ddacdbc-fba5-4298-9f8d-90b7ab937844",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:03Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "270fe471-7761-411c-a5c8-8aef5d50b090",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "26e75a55-97b5-4ec0-a061-74460a26659d",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:02Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "9f172d90-39e1-46ba-9271-e18d349f22ff",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "f25d2e8c-bf82-4cb5-9a80-a72bd83d85cf",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:01Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "0d98546c-6b0b-4d0c-a73c-68059eb76792",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "fd5300fb-d315-4ed3-b9e7-ca1b92a5d394",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:59Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "44bae06a-b763-4952-8832-41fc6ad7302c",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "22af1364-f2e4-41eb-bb18-f1738e807acf",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:58Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "af97f2a9-e028-4735-a6c6-9124b6679d5d",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "12794adb-6096-4389-9756-e98a5dca6d67",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:57Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "b3335448-07b5-4095-982d-b1b34a832ec5",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "19e72b5f-adba-48cc-ab37-53756ed926d5",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:56Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "a057578e-d65b-43a5-bb03-9914d7e1d069",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "a5578e6e-e935-4b5f-9d9e-7af60f7999e4",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:54Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "8ea8e04e-b423-4651-878a-c81a60213c16",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "a7175b36-d81e-4865-be81-212ca57308df",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:53Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "09a037ea-6fe5-4df3-bfeb-62c2de373b83",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "5bdf2db7-edd7-42cd-82f1-ee0196606656",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:52Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "6fb104db-448f-4055-b30c-c72cdc9cabcc",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "03ba7d84-509a-4bb9-bc48-959aa989b5ff",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "796082ea-1ed9-422e-8316-c8696499cd1e",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "a29037ea-ed15-4025-9a54-ff70f11ca95c",
	      "eventName": "DescribeInstanceInformation",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:18:49Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "294599468799",
	      "requestID": "5f7f7d07-7c66-41aa-8fb8-dacd955626df",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "InstanceIds",
	               "values": [
	                  "i-d3720C7af6fCfF2B2",
	                  "i-d0b6DCBA8984dE148",
	                  "i-eA1d1296c1dE3Aa1f"
	               ]
	            }
	         ]
	      },
	      "responseElements": null,
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "f8f0460c-476b-42b7-9cfb-cd6345e2aad1",
	      "eventName": "TerminateSession",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "294599468799",
	      "requestID": "9147312c-7312-46d4-aa91-798728055424",
	      "requestParameters": {
	         "sessionId": "christophe-wzleysigzmbd6fmkefjqvt5w4u"
	      },
	      "responseElements": {
	         "sessionId": "christophe-wzleysigzmbd6fmkefjqvt5w4u"
	      },
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "8086b250-d29c-4659-9aec-86c8446a3895",
	      "eventName": "StartSession",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "294599468799",
	      "requestID": "d81b3311-b5aa-4782-ab43-c7af5e237aee",
	      "requestParameters": {
	         "target": "i-eA1d1296c1dE3Aa1f"
	      },
	      "responseElements": {
	         "sessionId": "christophe-wzleysigzmbd6fmkefjqvt5w4u",
	         "streamUrl": "wss://ssmmessages.me-northwest-3r.amazonaws.com/v1/data-channel/christophe-wzleysigzmbd6fmkefjqvt5w4u?role=publish_subscribe\u0026cell-number=AAEAAbIWRNYnEkrB64bhGiedJQR3zYzBwUJyTNxc854+f3IBAAAAAGarfUW5QwfI91t6LkgX/EqdDx6EluDPvaUGK2bMPeDUpZ8JCNDVkDD7",
	         "tokenValue": "Value hidden due to security reasons."
	      },
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "131c198f-7042-4c88-be71-545471d55f4c",
	      "eventName": "TerminateSession",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "294599468799",
	      "requestID": "577db5d7-12b4-49a6-87eb-6ea2890065bd",
	      "requestParameters": {
	         "sessionId": "christophe-bkqs75qpcrtlxk5paaytrydm2e"
	      },
	      "responseElements": {
	         "sessionId": "christophe-bkqs75qpcrtlxk5paaytrydm2e"
	      },
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "10057a87-1da5-4c7d-a411-e41543dc91f5",
	      "eventName": "StartSession",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "294599468799",
	      "requestID": "5cc369d7-d3e9-41e0-a677-14e8c9c18c8e",
	      "requestParameters": {
	         "target": "i-d0b6DCBA8984dE148"
	      },
	      "responseElements": {
	         "sessionId": "christophe-s7uathgenk3m4qa2s33wio5gpu",
	         "streamUrl": "wss://ssmmessages.me-northwest-3r.amazonaws.com/v1/data-channel/christophe-s7uathgenk3m4qa2s33wio5gpu?role=publish_subscribe\u0026cell-number=AAEAASNZon/688w6/ZL2nfwe5JxliimfvbKltR2/CMq9mU3DAAAAAGarfUU7baqkmRTOTruWRhsNBxa9VYTF4cuEPM/a0XdVPGUYQNU1KAa3",
	         "tokenValue": "Value hidden due to security reasons."
	      },
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "60fd77a0-1ce9-40a1-b24b-0a598a169de9",
	      "eventName": "TerminateSession",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "294599468799",
	      "requestID": "ca9f1a4d-f89b-468d-9858-8e628165c8e7",
	      "requestParameters": {
	         "sessionId": "christophe-s7uathgenk3m4qa2s33wio5gpu"
	      },
	      "responseElements": {
	         "sessionId": "christophe-s7uathgenk3m4qa2s33wio5gpu"
	      },
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "me-northwest-3r",
	      "eventCategory": "Management",
	      "eventID": "32e8a07f-4751-4081-882e-958a25231c56",
	      "eventName": "StartSession",
	      "eventSource": "ssm.amazonaws.com",
	      "eventTime": "2024-08-01T12:19:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "294599468799",
	      "requestID": "bfa7688d-0e78-4252-b5f6-1a445c82f109",
	      "requestParameters": {
	         "target": "i-d3720C7af6fCfF2B2"
	      },
	      "responseElements": {
	         "sessionId": "christophe-bkqs75qpcrtlxk5paaytrydm2e",
	         "streamUrl": "wss://ssmmessages.me-northwest-3r.amazonaws.com/v1/data-channel/christophe-bkqs75qpcrtlxk5paaytrydm2e?role=publish_subscribe\u0026cell-number=AAEAAeHX0bqbU5dmbfb/NJVjO7TQopSahDHtyQVUjSI6yFXSAAAAAGarfUSzqvoBC+mhEuJQf0+1Y3iTcwzVAhL1LviE3BBll/7GdCowEhwg",
	         "tokenValue": "Value hidden due to security reasons."
	      },
	      "sourceIPAddress": "254.222.242.236",
	      "tlsDetails": {
	         "cipherSuite": "ECDHE-RSA-AES128-GCM-SHA256",
	         "clientProvidedHostHeader": "ssm.me-northwest-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.2"
	      },
	      "userAgent": "stratus-red-team_ae66c4b1-50c7-490d-b027-3a699952bd6a",
	      "userIdentity": {
	         "accessKeyId": "AKIA4HNRH6OJUWNZ893Z",
	         "accountId": "294599468799",
	         "arn": "arn:aws:iam::294599468799:user/christophe",
	         "principalId": "AIDAH36QPLPPPZVXSD3V",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   }
	]
    ```

[^1]: These logs have been gathered from a real detonation of this technique in a test environment using [Grimoire](https://github.com/DataDog/grimoire), and anonymized using [LogLicker](https://github.com/Permiso-io-tools/LogLicker).
