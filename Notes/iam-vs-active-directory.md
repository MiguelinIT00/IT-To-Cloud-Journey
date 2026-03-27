# AWS IAM vs Microsoft Active Directory
## Question  
What are the similarities between AWS IAM and Microsoft Active Directory?
## Answer  
AWS IAM (Identity and Access Management) and Microsoft Active Directory both serve a similar core purpose: managing identities and controlling access to resources. While they operate in different environments (cloud vs on-prem), they share a lot of foundational concepts.
## Similarities  
### User Management  
Both systems allow you to create and manage users, whether they are actual people or service accounts.
### Authentication  
Both verify identity using credentials such as usernames and passwords (and can support more advanced methods like MFA).
### Authorization  
Both control what users can access by assigning permissions, roles, or policies.
### Group-Based Access Control  
Users can be placed into groups, making it easier to assign and manage permissions at scale.
### Security Focus  
Both follow security best practices like least privilege, ensuring users only have access to what they need.
## Key Difference  
- **AWS IAM** is designed for managing access to AWS cloud resources (EC2, S3, etc.)  
- **Active Directory** is primarily used for managing users and devices in on-premise or corporate network environments  
## Notes  
This helped me understand that IAM is essentially AWS’s version of identity and access control, similar to what Active Directory does in traditional IT environments.
