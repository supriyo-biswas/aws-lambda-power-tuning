# AWS Lambda Power Tuning

AWS Lambda Power Tuning is a state machine powered by AWS Step Functions that helps you optimize your Lambda functions for cost and/or performance in a data-driven way.

The state machine is designed to be easy to deploy and fast to execute. Also, it's language agnostic so you can optimize any Lambda functions in your account.

Basically, you can provide a Lambda function ARN as input and the state machine will invoke that function with multiple power configurations (from 128MB to 10GB, you decide which values). Then it will analyze all the execution logs and suggest you the best power configuration to minimize cost and/or maximize performance.

## Differences with upstream

This fork is a bit different from the upstream project. It does not discard outliers to detect performance inconsistencies arising out of Lambda's container filesystem caching and cold starts.

## What does the state machine look like?

It's pretty simple and you can visually inspect each step in the AWS management console.

![state-machine](imgs/state-machine-screenshot.png?raw=true)

## What results can I expect from Lambda Power Tuning?

The state machine will generate a visualization of average cost and speed for each power configuration.

For example, this is what the results look like for two CPU-intensive functions, which become cheaper AND faster with more power:

![visualization1](imgs/visualization1.jpg?raw=true)

How to interpret the chart above: execution time goes from 35s with 128MB to less than 3s with 1.5GB, while being 14% cheaper to run.

![visualization2](imgs/visualization2.jpg?raw=true)

How to interpret the chart above: execution time goes from 2.4s with 128MB to 300ms with 1GB, for the very same average cost.


## How to deploy the state machine 

There are a few options documented [here](README-DEPLOY.md).


## How to execute the state machine

You can execute the state machine manually or programmatically, see the documentation [here](README-EXECUTE.md).


## State Machine Input and Output

Here's a typical execution input with basic parameters:

```json
{
    "lambdaARN": "your-lambda-function-arn",
    "powerValues": [128, 256, 512, 1024],
    "num": 50,
    "payload": {}
}
```

Full input documentation [here](README-INPUT-OUTPUT.md#user-content-state-machine-input).

The state machine output will look like this:

```json
{
  "results": {
    "power": "128",
    "cost": 0.0000002083,
    "duration": 2.906,
    "stateMachine": {
      "executionCost": 0.00045,
      "lambdaCost": 0.0005252,
      "visualization": "https://lambda-power-tuning.show/#<encoded_data>"
    }
  }
}
```

Full output documentation [here](README-INPUT-OUTPUT.md#user-content-state-machine-output).


## Data visualization

You can visually inspect the tuning results to identify the optimal tradeoff between cost and performance.

![visualization](imgs/visualization.png?raw=true)

The data visualization tool has been built by the community: it's a static website deployed via AWS Amplify Console and it's free to use. If you don't want to use the visualization tool, you can simply ignore the visualization URL provided in the execution output. No data is ever shared or stored by this tool.

Website repository: [matteo-ronchetti/aws-lambda-power-tuning-ui](https://github.com/matteo-ronchetti/aws-lambda-power-tuning-ui)

Optionally, you could deploy your own custom visualization tool and configure the CloudFormation Parameter named `visualizationURL` with your own URL.

## Power Tuner UI

Lambda Power Tuner UI is a web interface to simplify the deployment and execution of Lambda Power Tuning. It's built and maintained by [Matthew Dorrian](https://twitter.com/DorrianMatthew) and it aims at providing a consistent interface and a uniform developer experience across teams and projects.

![Power Tuner UI](https://github.com/mattymoomoo/aws-power-tuner-ui/blob/master/imgs/website.png?raw=true)

Power Tuner UI repository: [mattymoomoo/aws-power-tuner-ui](https://github.com/mattymoomoo/aws-power-tuner-ui)

## Additional features, considerations, and internals

[Here](README-ADVANCED.md) you can find out more about some advanced features of this project, its internals, and some considerations about security and execution cost.
