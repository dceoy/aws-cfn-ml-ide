aws-cfn-ml-ide
==============

AWS CloudFormation stacks of IDE for machine learning

[![Lint](https://github.com/dceoy/aws-cfn-ml-ide/actions/workflows/lint.yml/badge.svg)](https://github.com/dceoy/aws-cfn-ml-ide/actions/workflows/lint.yml)

Installation
------------

1.  Check out the repository.

    ```sh
    $ git clone --recurse-submodules git@github.com:dceoy/aws-cfn-ml-ide.git
    $ cd aws-cfn-ml-ide
    ```

2.  Install [Rain](https://github.com/aws-cloudformation/rain) and [AWS CLI](https://aws.amazon.com/cli/), and set `~/.aws/config` and `~/.aws/credentials`.

3.  Deploy stacks of VPC private subnets and a VPC endpoint for S3.

    ```sh
    $ rain deploy \
        --params ProjectName=ml-dev \
        aws-cfn-vpc-for-slc/vpc-private-subnets-with-gateway-endpoints.cfn.yml \
        ml-dev-vpc-private-subnets-with-gateway-endpoints
    ```

4.  Deploy stacks of S3 and IAM for SageMaker.

    ```sh
    $ rain deploy \
        --params ProjectName=ml-dev \
        s3-bucket-for-sagemaker.cfn.yml ml-dev-s3-bucket-for-sagemaker
    $ rain deploy \
        --params ProjectName=ml-dev,S3StackName=ml-dev-s3-bucket-for-sagemaker \
        iam-roles-for-sagemaker.cfn.yml ml-dev-iam-roles-for-sagemaker
    ```

5.  Deploy stacks of SageMaker Studio or SageMaker Notebook.

    - SageMaker Studio

      ```sh
      $ rain deploy \
          --params ProjectName=ml-dev,VpcStackName=ml-dev-vpc-private-subnets-with-gateway-endpoints,IamStackName=ml-dev-iam-roles-for-sagemaker \
          sagemaker-studio-domain.cfn.yml ml-dev-sagemaker-studio-domain
      $ rain deploy \
          --params ProjectName=ml-dev,SageMakerStudioDomainStackName=ml-dev-sagemaker-studio-domain,IamStackName=ml-dev-iam-roles-for-sagemaker \
          sagemaker-studio-user-profile.cfn.yml \
          ml-dev-sagemaker-studio-user-profile
      ```

    - SageMaker Notebook

      ```sh
      $ rain deploy \
          --params ProjectName=ml-dev,VpcStackName=ml-dev-vpc-private-subnets-with-gateway-endpoints,IamStackName=ml-dev-iam-roles-for-sagemaker \
          sagemaker-notebook.cfn.yml ml-dev-sagemaker-notebook
      ```

6.  Deploy stacks of VPC public subnets and a NAT gateway for internet access. (optional)

    ```sh
    $ rain deploy \
        --params VpcStackName=ml-dev-vpc-private-subnets-with-gateway-endpoints,ProjectName=ml-dev \
        aws-cfn-vpc-for-slc/vpc-public-subnets-with-nat-gateway-per-az.cfn.yml \
        ml-dev-vpc-public-subnets-with-nat-gateway-per-az
    ```
