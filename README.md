<!-- ---
marp: true
theme: default
paginate: true
--- -->
# aws-static-website-cloudformation

Deploy a static Website in AWS S3 as Proof of Concept, using CloudFormation.
Eventually implent dynamic features like CI/CD or serverless guestbook/contact form.

This can serve as pay-per-use template for your static Websites hostet on AWS.

## purpose

A minimalistic CloudFormation template, following Amazons [Guide][guide] for **hosting static websites**, without their overwhelming [500+ lines template][template].

- Minimalistic CloudFormation example for
- provisioning static webspace using
- aws s3 bucket
    - unblocked access
    - attached Read Policies
    - configured as website serving index.html by default

## target audience

- AWS Cloud Practitioners seeking hands-on AWS experience
- AWS CloudFormation learners or starters
- Webmasters looking for static pay-as-you-go webspace that later can be highly extended and scaled.

## AWS services used

- AWS CloudFormation Stacks
    - provision s3 storage
    - attach Policy
    - drift detection (optional)
- AWS S3 - Simple Storage Service
    - s3 can be used for backup, backend storage, versioned repositories, and even DataLakes.
    - it also serves as webspace for CloudFront and CDNs
    - it can also be configured as standlone-webspace, which is our scope
- AWS SDK (cli) (optional)
- AWS CloudShell (optional)

## provisioning and deployment

You need an aws account, could also be free tier.
- login to aws
- head to CloudFormation Stacks
- Upload [s3.cf.yml](./cloudformation/s3.cf.yml)
- choose an AppName (the Bucket name will be created from it):
`BucketName: !Sub "${AppName}-${AWS::Region}-${AWS::AccountId}"`
- deploy the Stack in the desired Region
- wait for the blue Stack State to turn green CREATION_COMPLETED
- see the Stack Output for information
    - S3 bucket name - you can upload your website here
    - SiteUrl - feel free to test from a browser that is NOT logged in to aws

## programmatic deployment

- install aws cli and git
- login `aws login`
    - orw create an API on the MMC key using CloudShell:
    `aws iam create-access-key` copy and paste into your ~/.aws/credentials
- clone [repo][repolink]: `git clone https://github.com/Codingschule/aws-static-website-cloudformation.git`
- cd into the dictionary
`cd aws-static-website-cloudformation`
- upload the Stack (change region)
`aws cloudformation deploy --template-body 'file://cloudformation/s3.cf.yml' --region=us-east-1 --stack-name RandomStackName`
instead **deploy** you can use **create-stack** or **update-stack** to be more specific.

## website url

If your AppName is `cs-itpro``and you deploy on us-east-1 using account 313333333337 your URL would be 
http://cs-itpro-s3web-us-east-1-313333333337.s3-website-us-east-1.amazonaws.com/

## cost calculation examples

Since inbound traffic is usually free and you only use an S3 bucket (default tarrif), the estimated costs depend on the size of the website, the number of files involed, and frequency of the website be requested.

- uploading / updating the website: usually low cost compared to outbound
- storage: storage actually used
- outbound traffic:
    - number of requests
    - data transfered out of aws

The following examples were calculated for us-east-1 on Jan 27th 2026 and are mere examples.
They give you an idea how cost changes with file **size** but also **number** of files.

### Cost for 1kB Website (1 file), called 1mio times, updated daily

| Category                           | Description                   | Calculation                | Cost (USD) |
| ----------------------------------- | ------------------------------ | ------------------------- | ------------ |
| **S3 Std – storage**          | Tiered price                   | 0,000001 GB × 0,023 USD   | 0,00         |
| **S3 Std – PUT**      | 30 PUT Requests                | 30 × 0,000005 USD         | 0,0001       |
| **S3 Std – GET**      | 1.000.000 GET Requests         | 1.000.000 × 0,0000004 USD | 0,40         |
| **S3 Select – transfer**         | Datenrücksendung               | 1 GB × 0,0007 USD         | 0,0007       |
| **monthly cost**                   | Storage + Requests + S3 Select | 0,40 + 0,0001 + 0,0007    | **0,40**     |


---

### Cost for 10 MB Website (stored as 1 file), completely requested 1mio times

| Category                                   | Description                      | Calculation                    | Cost (USD) |
| ------------------------------------------ | -------------------------------- | ------------------------------ | ---------- |
| **S3 Std – Storage**                  | Tiered price                     | 0.01 GB × 0.023 USD            | 0.00       |
| **S3 Std – Storage (tier)**     | Total tier cost                  | –                              | 0.0002     |
| **S3 Std – PUT**             | 30 PUT requests                  | 30 × 0.000005 USD              | 0.0001     |
| **S3 Std – GET**             | 1,000,000 GET | 1,000,000 × 0.0000004 USD      | 0.40       |
| **S3 Select – Data Returned**              | Data return                      | **10,240 GB × 0.0007** USD         | 7.168      |
| **Total (Storage + Requests + S3 Select)** | Combined monthly cost            | 0.0002 + 0.40 + 0.0001 + 7.168 | **7.57**   |
| **Monthly Cost (S3 Std)**             | Total monthly cost               | –                              | **7.57**   |

---

### Cost of 10 MB Site (100 files x 0.1 MB), each requested 1mio times

| Category                                   | Description                        | Calculation                    | Cost (USD) |
| ------------------------------------------ | ---------------------------------- | ------------------------------ | ---------- |
| **S3 Std – Storage**                  | Tiered price                       | 0.01 GB × 0.023 USD            | 0.00       |
| **S3 Std – Storage**     | Total tier cost                    | –                              | 0.0002     |
| **S3 Std – PUT**             | 3,000 PUT requests                 | 3,000 × 0.000005 USD           | 0.015      |
| **S3 Std – GET**             | 100 Mio GET  | **100 mio × 0.0000004** USD    | **40.00**      |
| **S3 Select – Data**              | Data return                        | 10,240 GB × 0.0007 USD         | 7.168      |
| **Total** | Combined              | 0.0002 + 40.00 + 0.015 + 7.168 | **47.18**  |


See [calculate.aws][calc] for detailed calculations

---

## Business opportunities and risks using AWS for website hosting

AWS S3 storage starts cheap and scales with cost.

- Low Traffic
    - You pay only storage - no visitors, no traffic, almost no cost
- Unexpected High Traffic 
    - High costs
    - No fear of outtage and reputation loss
    - Eventually more Customers generating revenue


## limitations and outlook

- add CloudFront for HTTPS transport encryption
- no custom sub/domain without CloudFront # TODO
- dynamic content requires compute services and/or databases
- no content limitation to verified/registered users

## disclaimer and known risks

This template comes "as it is" with out any warranty for completeness.
You also must be aware, that a serverless pay-as-you-go service like s3 - especially when configured as public space - because of is elasticity - can generate high costs if your website is downloaded often.
This might be intended by high-scale companies but could generate existential risks for individuals.
Use aws budget notifications, free tier without payment details, and other tools to protect yourself whilst testing.

---

## Autor
- [Matthias Block][matt]: Content
- [Sam Dillenburg][sam]: Mentoring
- Your favourite LLM: Confusion

[guide]: https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html "AWS Guide for hosting static websites on s3"
[Template]: https://github.com/aws-cloudformation/aws-cloudformation-templates/blob/main/S3/compliant-static-website.yaml "complete compliant-static-website.yaml"
[repolink]: https://github.com/Codingschule/aws-static-website-cloudformation "Internal link to this repository"
[calc]: https://calculator.aws/ "AWS cost calculator"
[matt]: https://github.com/yasuoiwakura "Matthias Block" 
[sam]: https://github.com/hackbraten68 "Sam Dillenburg"
