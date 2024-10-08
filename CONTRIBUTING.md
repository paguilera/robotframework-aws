# Development environment

Feel free to create issues for ideas for new functionality with other aws services

We are working to create an environment with tests in localstack https://github.com/localstack/localstack if you have 
experience with this tool and time, any help will be appreciated.

## Contributing to RobotFramework-AWS

Thank you for considering contributing to a library for interacting with AWS services in RobotFramework 
for test automation.

Configure your environment as desired, the requirements are in the requirements.txt file

```sh
pip install -r requirements.txt
```

### Keywords
All keyword names must start with the module name for easy searching in html documentation, example:
```robotframework
S3 Upload File
Sqs Send Message
CloudWatch Wait For Logs
Dynamo Query Table
```
### Versioning
From left to right. The first number is incremented every time a new AWS module is incorporated, that is, 
when a new python file with a new class is incorporated into the project.

The second one is incremented every time a new keyword is added to the existing modules. (sqs, dynamo, s3...)

The last one is incremented by bugfix or code improvements.

## Testing

### Localstack

For now, we have inside the folder localstack the docker compose file and in the init-aws.sh the commands to send
for localstack.

To start localstack just run inside localstack folder:
```sh
docker-compose up -d
```
Then you can use inside robot, the endpoint http://localhost:4566
see some examples in /tests/robot folder

I only tested this docker on Ubuntu, if you use other system and got some error, fell free to fix and send some pull request.

### Robot Framework

The tests suites are inside tests/robot folder

The common variables, keywords and libraries should be in common.resource file. The tests suites have the 
aws module name like s3.robot or sqs.robot then we can run it separately.

Any extra file like txt, json, csv or similar should be inside data folder.

The robot files should import only the common.resource.

```robotframework
*** Settings ***
Resource    common.resource
Suite Setup    Create Session And Set Endpoint
Suite Teardown    Delete All Sessions
```

All the libraries import should be in common.resource, as the suite setup. The import to AWSLibrary is relative to 
project source, because then you can test the new keywords or changes just running locally the tests

```robotframework
*** Settings ***
Library    ${CURDIR}/../../src/AWSLibrary
Library    Collections
Library    OperatingSystem


*** Keywords ***
Create Session And Set Endpoint
    Create Session With Keys    ${REGION}    ${ACCESS_KEY}    ${SECRET_KEY}
    SQS Set Endpoint Url    http://localhost:4566    # Point to localstack sqs instance
    S3 Set Endpoint Url    http://localhost:4566    # Point to localstack s3 instance
```

To run with debug level all tests, from the main folder:
```sh
robot -d log -L TRACE tests/robot
```

To run just one module, like s3:
```sh
robot -d log -L TRACE tests/robot/s3.robot
# or
robot -d log -L TRACE -i s3 tests/robot
```

### GitHub Actions

If you don´t have the environment to test the Library, you can fork the project and create the branch inside the folder 
test. Like "test/my-branch" then after do a push the GitHub actions will start the tests.

### TO-DO

- [ ]  Add more services in library and in localstack
- [ ]  Add robot tests for this new services
