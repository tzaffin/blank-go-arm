
# Blank function (Go)

> For a detailed step-by-step guide on how to set this up, check out this [blog post](https://tech.autoscout24.com/blog/posts/go-lambda-on-arm/).

This is a fork of [blank-go](https://github.dev/awsdocs/aws-lambda-developer-guide/tree/main/sample-apps/blank-go) from AWS.

![Architecture](images/sample-blank-go.png)

The project source includes function code and supporting resources:

- `function` - A Golang function.
- `template.yml` - An AWS CloudFormation template that creates an application.
- `1-create-bucket.sh`, `2-deploy.sh`, etc. - Shell scripts that use the AWS CLI to deploy and manage the application.

Use the following instructions to deploy the sample application.

# Requirements
- [Go executable](https://golang.org/dl/).
- The Bash shell. For Linux and macOS, this is included by default. In Windows 10, you can install the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows-integrated version of Ubuntu and Bash.
- [The AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) v1.17 or newer.

If you use the AWS CLI v2, add the following to your [configuration file](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) (`~/.aws/config`):

```
cli_binary_format=raw-in-base64-out
```

This setting enables the AWS CLI v2 to load JSON events from a file, matching the v1 behavior.

# Setup
Download or clone this repository.

    $ git clone https://github.com/awsdocs/aws-lambda-developer-guide.git
    $ cd aws-lambda-developer-guide/sample-apps/blank-go

To create a new bucket for deployment artifacts, run `1-create-bucket.sh`.

    blank-go$ ./1-create-bucket.sh
    make_bucket: lambda-artifacts-a5e491dbb5b22e0d

# Deploy

To deploy the application, run `2-deploy.sh`.

    blank-go$ ./2-deploy.sh
    Successfully packaged artifacts and wrote output template to file out.yml.
    Waiting for changeset to be created..
    Successfully created/updated stack - blank-go

This script uses AWS CloudFormation to deploy the Lambda functions and an IAM role. If the AWS CloudFormation stack that contains the resources already exists, the script updates it with any changes to the template or function code.

# Test
To invoke the function, run `3-invoke.sh`.

    blank-go$ ./3-invoke.sh
    {
        "StatusCode": 200,
        "ExecutedVersion": "$LATEST"
    }
    "{\"FunctionCount\":42,\"TotalCodeSize\":361861771}"

Let the script invoke the function a few times and then press `CRTL+C` to exit.

The application uses AWS X-Ray to trace requests. Open the [X-Ray console](https://console.aws.amazon.com/xray/home#/service-map) to view the service map.

![Service Map](images/blank-go-servicemap.png)

Choose a node in the main function graph. Then choose **View traces** to see a list of traces. Choose any trace to view a timeline that breaks down the work done by the function.

![Trace](images/blank-go-trace.png)

# Cleanup
To delete the application, run `4-cleanup.sh`.

    blank$ ./4-cleanup.sh
