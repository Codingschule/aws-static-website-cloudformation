---
marp: true
theme: default
paginate: true
---
# aws-static-website-cloudformation

Deploy a static Website in AWS S3 as Proof of Concept, using CloudFormation.
Eventually implent dynamic features like CI/CD or serverless guestbook/contact form.

This can serve as pay-per-use template for your static Websites hostet on AWS.

---

## purpose

A minimalistic CloudFormation template, following Amazons [Guide][guide] for **hosting static websites**, without their overwhelming [500+ lines template][template].

- Minimalistic CloudFormation example for
- provisioning static webspace using
- aws s3 bucket
    - unblocked access
    - attached Read Policies
    - configured as website serving index.html by default

---

## target audience

- AWS Cloud Practitioners seeking hands-on AWS experience
- AWS CloudFormation learners or starters
- Webmasters looking for static pay-as-you-go webspace that later can be highly extended and scaled.

---

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

---

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

---

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

---

## cost calculation example

Since inbound traffic is usually free and you only use an S3 bucket (default tarrif)

---

## limitations

- no https transport encryption without CloudFront
- no custom sub/domain without CloudFront # TODO
- no dynamic content (3rd party could be utilized)
- no content limitation to verified/registered users
- 

---

## disclaimer and known risks

This template comes "as it is" with out any warranty for completeness.
You also must be aware, that a serverless pay-as-you-go service like s3 - especially when configured as public space - because of is elasticity - can generate high costs if your website is downloaded often.
This might be intended by high-scale companies but could generate existential risks for individuals.
Use aws budget notifications, free tier without payment details, and other tools to protect yourself whilst testing.

---

## links

[guide]: https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html "AWS Guide for hosting static websites on s3"
[Template]: https://github.com/aws-cloudformation/aws-cloudformation-templates/blob/main/S3/compliant-static-website.yaml "complete compliant-static-website.yaml"
[repolink]: https://github.com/Codingschule/aws-static-website-cloudformation "Internal link to this repository"
