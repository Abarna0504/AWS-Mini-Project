# 🌐 Serverless Contact Form Using AWS

A simple **serverless contact form** built using AWS services.  
Users can submit their name, email, and message — automatically sent to the admin via **Amazon SES**, without any traditional servers.

---

## ☁️ AWS Services Used
- **S3** – Hosts static website (HTML form)  
- **API Gateway** – Handles HTTPS POST request  
- **Lambda** – Processes form data  
- **SES** – Sends email notifications  
- **CloudWatch** – Logs and monitoring  
- **IAM** – Secure permissions for Lambda  

---

## ⚙️ Architecture Flow
1. User submits form → hosted on **S3**  
2. **API Gateway** triggers **Lambda**  
3. **Lambda** uses **SES** to send email  
4. **CloudWatch** monitors all logs  

---

## 💻 Lambda Code (Python 3.9)
```python
import boto3, json
FROM_EMAIL = "abarnakanmani54@gmail.com"
TO_EMAIL = "bala.aparna5@gmail.com"
ses = boto3.client("ses", region_name="ap-south-1")

def lambda_handler(event, context):
    body = json.loads(event["body"])
    ses.send_email(
        Source=FROM_EMAIL,
        Destination={"ToAddresses": [TO_EMAIL]},
        Message={
            "Subject": {"Data": "New Contact Form Submission"},
            "Body": {"Text": {"Data": str(body)}}
        }
    )
    return {"statusCode": 200, "body": json.dumps({"message": "Email sent!"})}
