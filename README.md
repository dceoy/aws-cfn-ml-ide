aws-cfn-ml-lab
==============

AWS CloudFormation stacks of machine learning

[![Lint](https://github.com/dceoy/aws-cfn-ml-lab/actions/workflows/lint.yml/badge.svg)](https://github.com/dceoy/aws-cfn-ml-lab/actions/workflows/lint.yml)

Installation
------------

1.  Check out the repository.

    ```sh
    $ git clone git@github.com:dceoy/aws-cfn-ml-lab.git
    $ cd aws-cfn-ml-lab
    ```

2.  Install [Rain](https://github.com/aws-cloudformation/rain) and set `~/.aws/config` and `~/.aws/credentials`.

3.  Deploy stacks for SageMaker Studio.

    ```sh
    $ rain deploy sagemaker-studio-domain.cfn.yml sagemaker-studio-domain
    ```
