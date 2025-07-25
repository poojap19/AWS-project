project/
├── lambda_function.py    # AWS Lambda function (Python)
├── index.html            # Front-end HTML form
├── script.js             # JavaScript to send data to API
└── README.md             # Project documentation


Lambda code:
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('registernation-table')

def lambda_handler(event, context):
    body = json.loads(event['body'])  # Parse incoming JSON data
    
    # Insert data into DynamoDB
    table.put_item(
        Item={
            'email': body['email'],
            'name': body['name'],
            'phone': body['phone'],
            'password': body['password']
        }
    )

    # Return response
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps({'message': 'Registration successful'})
    }

