aws-cfn-ml-lab
==============

AWS CloudFormation stacks of machine learning

[![Lint](https://github.com/dceoy/aws-cfn-ml-lab/actions/workflows/lint.yml/badge.svg)](https://github.com/dceoy/aws-cfn-ml-lab/actions/workflows/lint.yml)

Installation
------------

1.  Check out the repository.

    ```sh
    $ git clone --recurse-submodules git@github.com:dceoy/aws-cfn-ml-lab.git
    $ cd aws-cfn-ml-lab
    ```

2.  Install [Rain](https://github.com/aws-cloudformation/rain) and [AWS CLI](https://aws.amazon.com/cli/), and set `~/.aws/config` and `~/.aws/credentials`.

3.  Deploy stacks of VPC private subnets and a VPC endpoint for S3.

    ```sh
    $ rain deploy \
        --params ProjectName=mllab-dev \
        aws-cfn-vpc-for-slc/vpc-private-subnets-and-s3-endpoint.cfn.yml \
        mllab-dev-vpc-private
    ```

4.  Deploy stacks of SageMaker Studio.

    ```sh
    $ rain deploy \
        --params ProjectName=mllab-dev \
        s3-bucket-for-sagemaker.cfn.yml \
        mllab-dev-sagemaker-s3-bucket
    $ rain deploy \
        --params ProjectName=mllab-dev,S3StackName=mllab-dev-sagemaker-s3-bucket \
        iam-roles-for-sagemaker.cfn.yml \
        mllab-dev-sagemaker-iam-roles
    $ rain deploy \
        --params ProjectName=mllab-dev,VpcStackName=mllab-dev-vpc-private,IamStackName=mllab-dev-sagemaker-iam-roles \
        sagemaker-studio-domain.cfn.yml \
        mllab-dev-sagemaker-studio-domain
    ```

5.  Deploy stacks of VPC public subnets and a Nat gateway for internet access. (optional)

    ```sh
    $ rain deploy \
        --params VpcStackName=mllab-dev-vpc-private,ProjectName=mllab-dev \
        aws-cfn-vpc-for-slc/vpc-public-subnets-and-nat-gateway.cfn.yml \
        mllab-dev-vpc-public
    ```
