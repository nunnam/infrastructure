# CI CD Pipelines using Github Action for Serverless Applications
* Github actions enabled the several serverless commands.
## Stages involved in CI and CD
* Installation - npm install to install node js dependencies
* Linting - Linters check your code for syntax errors and highlight errors to make sure you can quickly find and fix them. ESLint is a linter that you can integrate into your Visual Studio Code setup in order to ensure code integrity.
* Unit Testing - npm test is a shortened version of npm run test ; npm is running the test command as defined in the package. json configuration file.
* Sonar Scanning - Sonar does static code analysis, which provides a detailed report of bugs, code smells, vulnerabilities, code duplications
* Quality Gates - work in progress
* Code Coverage - A report is going to save as archive
* Deploy - serverless deploy to deploy the code to aws
* Integration tests

## Usage
```yaml
name: CI  
on:
  push:
    branches: [ main ]
jobs:
  node-build-publish:
    uses: nunnam/infrastructure/.github/workflows/ci.yml@main
    with: 
      nodejs-version: "18.x"
      unittest-npm-cmd: "test"
      aws-role-to-assume: "<role arn>"
      aws-region: "<region>"
      linting: true
      sonar-scanner-enabled: true
      serverless-version: "2.72.3"
      sls-deploy: true
      sls-config-file: <serverless yaml file name>
      integration-tests-cmd: "cmd from package.json"

```

## Flags

| **variable**                                               | `Required` | `Default Value` | `Comments` |
| --------------------------------------------------------------- | ------------------- | ---------------- | ------------------------- |
| nodejs-version |       False             |      18.x          |    required nodejs version to run the stages and build the application                       |
| aws-role-to-assume |       False             |               |  IAM Role To deploy in aws                     |
| aws-region |       False             |        eu-central-1       |  Provide the required region if not it will take the default region for session                         |
| linting |       False             |               |      To perform code snifing and linting                     |
| unittest-npm-cmd |       False             |               |    to perform unit tests on code, provide the cmd from package.json                      |
| sonar-scanner-enabled |       False             |               |     scan the code with node sonar scanner binary                      |
| serverless-version |       False             |        2.72.3       |    provide serverless version, otherwise default value will be taken                        |
| retention-days |       False             |        5      |    to keep the code coverage report in artifacts                       |
| artifact_path |       False             |        output/test/code-coverage.html      |  if code coverage report is generated it will be saved in the specified path                         |
| sls-deploy |       False             |              |    To deploy application to aws using serverless library,  aws role arn also required                    |
| sls-config-file |       False             |   serverless.yaml           |    Provide serverless config file path            |
| integration-tests-cmd |       False             |             |    Provide cmd from package.json file to run integration tests           |

## Secrets
| **variable**                                               | `Comments` |
| --------------------------------------------------------------- | ------------------------- |
| ENV_GITHUB_TOKEN|   required to upload sonar reports to sonarqube app                     | 
| SONAR_TOKEN |      required to upload report to sonarqube            |



