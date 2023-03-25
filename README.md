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
        --params ProjectName=mlide-dev \
        aws-cfn-vpc-for-slc/vpc-private-subnets-with-endpoints.cfn.yml \
        mlide-dev-vpc-private
    ```

4.  Deploy stacks of S3 and IAM for SageMaker.

    ```sh
    $ rain deploy \
        --params ProjectName=mlide-dev \
        s3-bucket-for-sagemaker.cfn.yml \
        mlide-dev-sagemaker-s3-bucket
    $ rain deploy \
        --params ProjectName=mlide-dev,S3StackName=mlide-dev-sagemaker-s3-bucket \
        iam-roles-for-sagemaker.cfn.yml \
        mlide-dev-sagemaker-iam-roles
    ```

5.  Deploy stacks of SageMaker Studio or SageMaker Notebook.

    - SageMaker Studio

      ```sh
      $ rain deploy \
          --params ProjectName=mlide-dev,VpcStackName=mlide-dev-vpc-private,IamStackName=mlide-dev-sagemaker-iam-roles \
          sagemaker-studio-domain.cfn.yml \
          mlide-dev-sagemaker-studio-domain
      $ rain deploy \
          --params ProjectName=mlide-dev,SageMakerStudioDomainStackName=mlide-dev-sagemaker-studio-domain \
          sagemaker-studio-user-profile.cfn.yml \
          mlide-dev-sagemaker-studio-user-profile
      ```

    - SageMaker Notebook

      ```sh
      $ rain deploy \
          --params ProjectName=mlide-dev,VpcStackName=mlide-dev-vpc-private,IamStackName=mlide-dev-sagemaker-iam-roles \
          sagemaker-notebook.cfn.yml \
          mlide-dev-sagemaker-notebook
      ```

6.  Deploy stacks of VPC public subnets and a NAT gateway for internet access. (optional)

    ```sh
    $ rain deploy \
        --params VpcStackName=mlide-dev-vpc-private,ProjectName=mlide-dev \
        aws-cfn-vpc-for-slc/vpc-public-subnets-with-nat-gateway-per-az.cfn.yml \
        mlide-dev-vpc-public
    ```
