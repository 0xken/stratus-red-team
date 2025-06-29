---
title: Retrieve a High Number of Secrets Manager secrets
---

# Retrieve a High Number of Secrets Manager secrets


 <span class="smallcaps w3-badge w3-blue w3-round w3-text-white" title="This attack technique can be detonated multiple times">idempotent</span> 

Platform: AWS

## Mappings

- MITRE ATT&CK
    - Credential Access



## Description


Retrieves a high number of Secrets Manager secrets, through secretsmanager:GetSecretValue.

<span style="font-variant: small-caps;">Warm-up</span>: 

- Create multiple secrets in Secrets Manager.

<span style="font-variant: small-caps;">Detonation</span>: 

- Enumerate the secrets through secretsmanager:ListSecrets
- Retrieve each secret value, one by one through secretsmanager:GetSecretValue

References:

- https://permiso.io/blog/lucr-3-scattered-spider-getting-saas-y-in-the-cloud
- https://unit42.paloaltonetworks.com/muddled-libra-evolution-to-cloud/


## Instructions

```bash title="Detonate with Stratus Red Team"
stratus detonate aws.credential-access.secretsmanager-retrieve-secrets
```
## Detection


Identify principals retrieving a high number of secrets, through CloudTrail's GetSecretValue event.

The following may be use to tune the detection, or validate findings:

- Principals who do not usually call secretsmanager:GetSecretValue
- Attempts to call GetSecretValue resulting in access denied errors


## Detonation logs <span class="smallcaps w3-badge w3-light-green w3-round w3-text-sand">new!</span>

The following CloudTrail events are generated when this technique is detonated[^1]:


- `secretsmanager:GetSecretValue`

- `secretsmanager:ListSecrets`


??? "View raw detonation logs"

    ```json hl_lines="6 39 72 105 138 171 204 237 270 303 336 377 410 443 476 509 542 575 608 641 674"

    [
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "b9c3d881-1e77-426c-abd3-5ca20d903380",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:52Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "c4fff253-825a-4828-adac-7f789f6975f3",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-18-4Rzn83"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "c63dd227-42e0-4934-8b29-52f4e583d54e",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:52Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "df133663-cdb1-4ea8-b795-eddf0152e16c",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-17-JF56OW"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "0985f4e9-9263-423a-a499-fdd330c973c1",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "cf234c05-2c74-49e5-b632-5898071d4f86",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-2-WNXFB1"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "25b97ad2-f713-4a29-af76-659e736629aa",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "52b87720-e08a-4fd4-8daa-ad70f983ce68",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-14-3JB2S0"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "2d81c956-58c3-4336-ae4e-c0b9f2b96113",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "999b3685-f5e1-4008-9cc8-b83121ab679e",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-9-BHrKxX"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "853be248-0703-49a6-ba35-256dfbac47ab",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "6f25a056-21bc-4dc0-b19f-ebd556481158",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-7-WNXFB1"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "da3b695c-bf67-4648-af49-2bdfee197c14",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "9d284480-aa0c-4629-ad39-a99aa008322b",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-8-jLR7H1"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "8d045085-7bad-401a-9a04-4feba3f1073e",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:49Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "a34ac8a9-1314-42b5-abf7-1fde8260e136",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-12-DyLJjP"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "a9b70c0d-d32d-41e7-8356-2be543095478",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:49Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "84c979ef-40a9-42d6-844c-a472d4d6a2ba",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-10-DyLJjP"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "dbff1b29-c7fb-4fe4-b5ed-24e8794b77fe",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:49Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "6d9c4644-a87b-46fa-b76f-2cc62f8f6f64",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-15-SAZN9Q"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "0451b1de-e314-437f-a18d-827565e02bc9",
	      "eventName": "ListSecrets",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:48Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "818f243c-bb6b-43b1-9701-5180eecc90d2",
	      "requestParameters": {
	         "filters": [
	            {
	               "key": "tag-key",
	               "values": [
	                  "StratusRedTeam"
	               ]
	            }
	         ],
	         "maxResults": 100
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "99207de2-f8ea-4160-bbe8-22cb14da3a26",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:48Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "3f6c8311-bc51-43cf-88b8-5e51f424c1fd",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-4-Rma50d"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "d92519e6-b907-4d3a-abb4-d63c9feaee52",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:48Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "b1c6113d-e471-456d-9841-c094e4b47618",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-19-fXrpF0"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "e20f1d5b-f2fa-470f-8d33-8aa43ddb6a23",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:48Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "6604143b-2af2-49d6-90bf-1520228a658a",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-1-fXrpF0"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "bd52f504-dd75-46cb-a14e-e447612ea736",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "ad14aa03-62ac-4e31-afbb-5bdd640e051e",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-0-28bajb"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "2e8dca5a-4e30-4feb-91bd-8a09cd1067a5",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "12e83f12-234a-4ed6-a8a2-49b68a54abde",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-16-JcCztd"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "304c3bc6-5daa-4405-bbee-e6c65d276c20",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "45167e35-4642-41ae-bb82-0c431ce5dd24",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-13-MNjL4W"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "43ebe9e4-8a82-4bd2-b5bc-bf9585c53bca",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "a3376683-89a2-4a39-b490-adeed0bd02c1",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-6-JcCztd"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "a05794ec-3c4c-43f6-b302-cce3f6abf05e",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "de58de72-13f4-4f0e-8b23-2f25717ca82b",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-5-fyShdO"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "c9efcd4d-a04b-4abe-8fb4-2d954bcfda77",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "c686956f-fd49-433d-bdc7-c2fe91012036",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-3-DyLJjP"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "cn-centraleast-1r",
	      "eventCategory": "Management",
	      "eventID": "879e946f-b912-44e3-9d82-a84ad0b06668",
	      "eventName": "GetSecretValue",
	      "eventSource": "secretsmanager.amazonaws.com",
	      "eventTime": "2024-07-31T12:36:49Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "903144391865",
	      "requestID": "7ab119ac-f938-4bcc-86e8-9917493ace97",
	      "requestParameters": {
	         "secretId": "arn:aws:secretsmanager:cn-centraleast-1r:903144391865:secret:stratus-red-team-retrieve-secret-11-OyGWSO"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "253.201.144.253",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "secretsmanager.cn-centraleast-1r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_10e7edf3-5063-4eba-8842-68f83bb52d65",
	      "userIdentity": {
	         "accessKeyId": "AKIA1JMTHE9ZMZMWG0MG",
	         "accountId": "903144391865",
	         "arn": "arn:aws:iam::903144391865:user/christophe",
	         "principalId": "AIDA45XCCHPPLELFTIIM",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   }
	]
    ```

[^1]: These logs have been gathered from a real detonation of this technique in a test environment using [Grimoire](https://github.com/DataDog/grimoire), and anonymized using [LogLicker](https://github.com/Permiso-io-tools/LogLicker).
