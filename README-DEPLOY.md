# Build and deploy with the AWS SAM CLI

1. Install the [AWS SAM CLI in your local environment](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html).

1. Configure your [AWS credentials (requires AWS CLI installed)](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration):
    ```bash
    $ aws configure
    ```
1. Clone this git repository: 
    ```bash
    $ git clone https://github.com/alexcasalboni/aws-lambda-power-tuning.git
    ```
1. Build the Lambda layer and any other dependencies:
    ```bash
    $ cd ./aws-lambda-power-tuning
    $ sam build -u
    ```
    [`sam build -u`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-build.html) will run SAM build using a Docker container image that provides an environment similar to that which your function would run in. SAM build in-turn looks at your AWS SAM template file for information about Lambda functions and layers in this project.
    
    Once the build has completed you should see output that states `Build Succeeded`. If not there will be error messages providing guidance on what went wrong.
1.  Deploy the application using the SAM deploy "guided" mode:
    ```bash
    $ sam deploy -g
    ```
    [`sam deploy -g`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) will provide simple prompts to walk you through the process of deploying the tool. Provide a unique name for the 'Stack Name' and supply the [AWS Region](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html#Concepts.RegionsAndAvailabilityZones.Regions) you want to run the tool in and then you can select the defaults for testing of this tool. After accepting the prompted questions with a "Y" you can optionally save your application configuration. 

    After that the SAM CLI will run the required commands to create the resources for the Lambda Power Tuning tool. The CloudFormation outputs shown will highlight any issues or failures.
    
    If there are no issues, once complete you will see the stack outputs and a `Successfully created/updated stack` message.

## How to execute the state machine once deployed?

See [here](README-EXECUTE.md).
