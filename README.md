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

All you need is an aws account, that could be free tier or even a KodeKloud AWS playground.
- login to aws
- head to CloudFormation Stacks

---

## programmatic deployment

---

## cost calculation

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
