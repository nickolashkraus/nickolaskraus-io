+++
title = 'Automatic Certificate Validation with Certificate Validator'
date = 2019-10-13T00:00:00-00:00
lastmod = 2025-08-22T00:00:00-00:00
draft = false
description = '''
Certificate Validator is an AWS CloudFormation custom resource which
facilitates AWS Certificate Manager (ACM) certificate validation via DNS.
'''
tags = ['aws', 'cloudformation', 'custom-resource']
aliases = ['/articles/automatic-certificate-validation-with-certificate-validator/']
+++

## Problem

Amazon has enabled entire companies to be created around specific shortcomings
in their platform. [Serverless][Serverless], a framework that facilitates
seamless deployments of AWS Lambda functions, addresses the chicken and egg
problem when deploying a CloudFormation stack that defines an AWS Lambda
function and the S3 bucket in which it resides.

Similarly, the process of creating and validating an AWS Certificate Manager
(ACM) certificate via CloudFormation is equally cumbersome. The
[`AWS::CertificateManager::Certificate`][AWS::CertificateManager::Certificate]
resource requests an ACM certificate that you can use to enable secure
connections. However, when you use the `AWS::CertificateManager::Certificate`
resource in an AWS CloudFormation stack, the stack will remain in the
`CREATE_IN_PROGRESS` state until you validate the certificate request. Further
stack operations will be delayed until acting upon the instructions in the
validation email or by adding a CNAME record to your DNS configuration.

Both options break a fully automated deployment pipeline as they require manual
intervention. Additionally, if email validation is used for a certificate, to
renew the certificate, a renewal email must be acted upon as well. If more than
72 hours have elapsed since you received the validation email, the certificate
will expire and you will have to request a new certificate. As browsers move
toward restricting access to domains not secured by TLS, this can have serious
consequences for a website, service or application that uses an ACM
certificate.

## Solution

As previously stated, an `AWS::CertificateManager::Certificate` resource can be
validated via email or DNS. Email validation is problematic, since it is
difficult to automate and introduces additional points of failure to a system.
DNS validation, however, has its own automation issues. One could manually
create the CNAME records in the AWS Management Console:

**Certificate Manager** > **Create record in Route 53**

but this defeats the purpose of automation. You could also write a script to
retrieve the CNAME record name and value from the CloudFormation stack events,
but this option is limited by the fact that AWS CloudFormation only returns the
CNAME record for the domain specified by the [`DomainName`][DomainName]
property of the `AWS::CertificateManager::Certificate` resource, so forget
about validating any additional fully qualified domain names (FQDNs) given
under the [`SubjectAlternativeNames`][SubjectAlternativeNames] property for the
certificate.

This is where Certificate Validator comes in.[^1]

## What is Certificate Validator?

[Certificate Validator][Certificate Validator] is an AWS CloudFormation custom
resource which facilitates ACM certificate validation via DNS. A custom
resource allows you to write custom provisioning logic in templates that AWS
CloudFormation runs anytime you create, update or delete stacks. A custom
resource is basically an abstraction of every concrete AWS CloudFormation
resource. However, you, the developer, are now responsible for its lifecycle.

At its core, Certificate Validator is an AWS Lambda function that handles the
creation and validation of an ACM certificate. It accomplishes this without
stopping the execution of the CloudFormation deployment and automatically
creates the record sets in Route 53 that are used to validate the ACM
certificate.

## Getting Started

It is extremely easy to get started with Certificate Validator. Simply deploy
Certificate Validator and add the `Custom::Certificate` and
`Custom::CertificateValidator` custom resources to your AWS CloudFormation
templates.

Check out the [Getting Started][Getting Started] documentation for a full
guide.

## Conclusion

Using custom resources, an enterprising developer can fill the gaps left by AWS
CloudFormation. Certificate Validator removes the manual process of clicking an
email or going out to the AWS Management Console to validate a certificate,
which increases automation and improves security and reliability.

Certificate Validator is fully open-source. If you would like to contribute to
its development, please read the [Contributing][Contributing] documentation. If
you would like to report bugs, request features or submit feedback, create an
[issue][issue].

## Update: 2025-08-22

Though this product never got any traction (I don't even think my company at
the time ever deployed it either), I learned a lot about CloudFormation custom
resources, which I was able to leverage on future projects.

I still maintain a fork of Certificate Validator, albeit archived, on my
personal GitHub [here][nickolashkraus/certificate-validator].

[^1]: Looking back, this is so quaint. DNS validation can be accomplished
trivially with Terraform.

[Serverless]: https://serverless.com
[AWS::CertificateManager::Certificate]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-certificatemanager-certificate.html
[DomainName]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-certificatemanager-certificate.html#cfn-certificatemanager-certificate-domainname
[SubjectAlternativeNames]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-certificatemanager-certificate.html#cfn-certificatemanager-certificate-subjectalternativenames
[Certificate Validator]: https://github.com/Dwolla/certificate-validator
[Getting Started]: https://github.com/Dwolla/certificate-validator/blob/master/docs/getting-started.md
[Contributing]: https://github.com/Dwolla/certificate-validator/blob/master/CONTRIBUTING.md
[issue]: https://github.com/Dwolla/certificate-validator/issues
[nickolashkraus/certificate-validator]: https://github.com/nickolashkraus/certificate-validator
