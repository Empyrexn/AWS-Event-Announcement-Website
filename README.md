# Event Announcement Website (Serverless AWS Architecture)

Disclaimer: This project was developed as an educational demonstration of serverless web architecture using AWS services. All configurations and examples provided here are for instructional purposes only and do not represent production infrastructure.

## Overview

This project focuses on building a serverless event announcement website that allows users to subscribe to event notifications via email, view upcoming events, and create new events through a web form. The solution is designed to be scalable, cost-effective, and fully managed, utilizing key AWS services to eliminate the need for traditional server management.

The frontend is hosted as a static website on Amazon S3, while backend processing is handled through AWS Lambda functions triggered by API Gateway endpoints. Amazon SNS is used to manage subscribers and send notifications whenever a new event is created.

![Architecture Diagram](https://github.com/user-attachments/assets/f4d34f6c-e349-4080-b2a6-106dc0cb2b5e)

## Key Components

1. **Amazon S3**
   - **Role:** S3 hosts the static website and stores event data in a JSON file (`events.json`). The frontend components—HTML, CSS, and JavaScript—are uploaded to an S3 bucket configured for static website hosting.
   - **Functionality:** Provides the public interface for users to interact with the website, view events, and submit new ones.
   - **Security:** Public read access is granted only for the website files. Write access is limited to authorized Lambda functions via IAM policies.

2. **Amazon API Gateway**
   - **Role:** API Gateway acts as the intermediary between the frontend and backend services. It exposes RESTful endpoints that trigger Lambda functions for subscribing users and creating events.
   - **Functionality:**  
     - `/subscribe` endpoint adds a new subscriber.  
     - `/create-event` endpoint registers a new event and updates the event data in S3.
   - **Security:** Each endpoint is integrated with its corresponding Lambda function using IAM roles, and CORS is enabled to allow browser-based interaction.

3. **AWS Lambda**
   - **Role:** Lambda provides serverless compute functionality to process API requests. Two Lambda functions handle subscription management and event registration.
   - **Functions:**  
     - **Subscription Lambda:** Adds subscriber emails to an SNS topic when users subscribe through the frontend.  
     - **Event Registration Lambda:** Updates `events.json` in S3 with new event data and publishes notifications to subscribers through SNS.
   - **Security:** IAM roles ensure that each function has permission to access only the necessary AWS services, such as S3 and SNS.

4. **Amazon SNS (Simple Notification Service)**
   - **Role:** SNS manages the list of subscribers and distributes notifications when new events are added.
   - **Functionality:**  
     - Stores subscriber emails confirmed through the subscription process.  
     - Sends broadcast email notifications when an event is created.
   - **Security:** Subscribers confirm their email addresses to prevent unauthorized additions. IAM roles restrict which resources can publish to the SNS topic.

5. **AWS IAM (Identity and Access Management)**
   - **Role:** IAM manages access and permissions for all services used in the project.
   - **Functionality:**  
     - Provides Lambda functions with least-privilege access to S3 and SNS.  
     - Secures API Gateway integration with Lambda through execution roles.
   - **Security:** IAM policies enforce the principle of least privilege, ensuring that no component has more access than required.

## Objectives

- **Serverless Design:** Implement a fully managed, serverless web application using AWS native services.  
- **Event Notifications:** Allow users to subscribe for email notifications when new events are created.  
- **Scalability:** Utilize Lambda and SNS to handle variable workloads automatically without manual scaling.  
- **Data Persistence:** Store event data securely in S3 while ensuring fast retrieval for the frontend.  
- **Cost Efficiency:** Operate entirely within the AWS Free Tier, minimizing infrastructure costs.

## Implementation Steps

1. **Set Up S3 Static Hosting**
   - Create an S3 bucket and upload `index.html`, `style.css`, and `events.json`.  
   - Enable static website hosting and make the files publicly accessible.

2. **Configure SNS**
   - Create an SNS topic to handle email subscriptions.  
   - Verify subscriber emails through the SNS confirmation process.

3. **Deploy Lambda Functions**
   - Create the **Subscription Lambda** and **Event Registration Lambda** functions.  
   - Assign appropriate IAM roles allowing access to S3 and SNS.  
   - Define environment variables for topic ARNs and bucket names.

4. **Set Up API Gateway**
   - Create a REST API with `/subscribe` and `/create-event` endpoints.  
   - Integrate each endpoint with its corresponding Lambda function.  
   - Enable CORS and deploy the API.

5. **Test the System**
   - Subscribe using a test email and confirm via SNS.  
   - Submit a new event and verify that `events.json` is updated in S3.  
   - Ensure subscribers receive email notifications.

## Security Considerations

- **Public Access Control:** S3 is public only for website content. All write operations are performed securely via Lambda.  
- **IAM Roles:** Lambda functions are granted least-privilege access for specific resources.  
- **Email Verification:** SNS confirms each email before adding it to the subscription list.  
- **Data Integrity:** S3 versioning and proper permissions protect event data from unauthorized modification.

## Testing and Validation

- Verified that event creation triggers an update to `events.json` in S3.  
- Confirmed email notifications are delivered successfully to subscribers.  
- Tested API endpoints with Postman and through the frontend form.  
- Validated proper IAM permissions for S3 and SNS access.

## Conclusion

This project demonstrates the use of AWS serverless services to create a scalable, event-driven web application. By integrating **S3**, **Lambda**, **API Gateway**, **SNS**, and **IAM**, it eliminates the need for traditional infrastructure management while maintaining security, reliability, and low operational costs.

![image](https://github.com/user-attachments/assets/19fbba84-2dc4-4fe3-af62-9b9617a3f5c9)
![image](https://github.com/user-attachments/assets/dcb1e62f-5044-432a-9869-7799ae8de62a)
![image](https://github.com/user-attachments/assets/88deac61-d42b-43ab-ae13-2e8c048683a9)
![image](https://github.com/user-attachments/assets/527ba2e9-5f98-41dc-8075-af7153687c7c)

