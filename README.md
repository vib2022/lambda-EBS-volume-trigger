# lambda-EBS-volume-trigger
As a Cloud Engineering team we take care of the AWS environment and make sure it is in compliance with the organizational policies.
We use AWS cloud watch in combination with AWS Lambda to govern the resources according to the policies.
For example, we Trigger a Lambda function when an Amazon Elastic Block Store (EBS) volume is created. We use Amazon CloudWatch Events. CloudWatch Events that allows us to monitor and respond to EBS volumes that are of type GP2 and convert them to type GP3.

#Below is the lambda code triggered after an EBS volume is created, then converts it to a gp3 volume

import json
import boto3


def get_volume_id_from_arn(volume_arn):
    arn_parts = volume_arn.split(':')
    #The Volume ID is the last part of the ARN
    volume_id = arn_parts[-1].split('/')[-1]
    return volume_id
    
    
    
def lambda_handler(event, context):
    volume_arn = event['resources'][0]
    volume_id = get_volume_id_from_arn(volume_arn)
    
    ec2_client = boto3.client('ec2')
    
    response = ec2_client.modify_volume(
        VolumeId = volume_id,
        VolumeType = 'gp3',
        )

