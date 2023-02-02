import json
import boto3
ses_client = boto3.client("ses", region_name="us-east-1")
CHARSET = "UTF-8"
def lambda_handler(event, context):
    body_email = []
    detail_type_str = "Headsup, Recieved an alert from {}\n".format(event['detail-type'])
    body_email.append(detail_type_str)
    
    account_id_str =  "The account is {}\n".format(event['account'])
    body_email.append(account_id_str)
    
    score_description = "The score is {} and Description is {}\n".format(event['detail']['severity']['score'], event['detail']['severity']['description'])
    body_email.append(score_description)
    
    s3_bucket_details = "The affected S3 bucket name is {} with arn {} created at {} and owned by {}".format(event['detail']['resourcesAffected']['s3Bucket']['name'], event['detail']['resourcesAffected']['s3Bucket']['arn'], event['detail']['resourcesAffected']['s3Bucket']['createdAt'], event['detail']['resourcesAffected']['s3Bucket']['owner']['displayName'], )
    body_email.append(s3_bucket_details)          
    
    for body_str in range(len(body_email)):
        print (body_email[body_str])
        
        response = ses_client.send_email(
            Destination={
                "ToAddresses": [
                    "you@email.com",
                ],
            },
            Message={
                "Body": {
                    "Html": {
                        "Charset": CHARSET,
                        "Data": json.dumps(body_email, indent=4)
                    }
                },
                "Subject": {
                    "Charset": CHARSET,
                    "Data": "MACIE ALERT",
                },
            },
            Source="you@email.com,
        )
