# Lets Encrypt Certificates 

WinAcme is dead. Using SimpleAcme instead.

## Setup for Route 53 Validation

Once you have installed the Acme client. Make sure you use the full version from here
https://simple-acme.com/download

Then install the Route53 plugin on the same page.

Find each zip in the download directory and uncheck the [ ] Unblock and apply before extracting the files.

Extract to the same directory as simple-acme (wacs.exe).

Create a user in AWS Iam and grant it your appropriate security options. Be as restrictive as you think. I created a specific user and applied this, but use your own judgement.

```JSON
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "UpdateRecords",
			"Effect": "Allow",
			"Action": [
				"route53:GetChange",
				"route53:ChangeResourceRecordSets",
				"route53:ListResourceRecordSets"
			],
			"Resource": [
				"arn:aws:route53:::hostedzone/*",
				"arn:aws:route53:::change/*"
			]
		},
		{
			"Sid": "ViewRecords",
			"Effect": "Allow",
			"Action": "route53:ListHostedZones",
			"Resource": "*"
		}
	]
}
```

### Add Secret
Add your Secret to the vault
(O) More Options -> (S) manage Secrets -> (A) Add Secret -> (enter the value) -> Give it a name For use in the next step.

### Edit Renewals
If you have existing renewals (as I did) you'll need to edit them.

(A) Manage Renewals -> select the number -> (E) Edit Renewal -> (5) Validation ->  "Find [dns] Create verification Records in Route 53 DNS" (mine was option 6) -> (3) Access Key -> Enter the Access Key ID (the non-secret piece) -> (2) Search in Vault -> (1) vault://json/(name from previous step) -> Assume STS - skip <enter> -> region <default>








