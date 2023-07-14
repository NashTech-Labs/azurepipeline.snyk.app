# Snyk App test template

This template consists of a yaml for Azure pipeline that can be extended as template for snyk app test.

## Pipeline Requirements

The Snyk Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| nodeVersion | Version of Node for Snyk | string | '20.x' | | Required ||
| snykServiceConnectionName  | Azure Service Connection for Snyk in your Azure pipeline | string | | | Required | Remember to provide permission to pipeline for the snyk token used |
| targetFile | Location of manifest file you require to be tested | string | | | Optional |  |
| severityThreshold | severity type to be used as threshold by snyk | string | 'always' |'always' / 'onIssuesFound' /'never' | Optional |  |
| monitorWhen | Condition to run Snyk monitor to capture dependency tree of the application | string | "always" | "always" / "onIssuesFound" / "never" | |
| failOnIssues | specifies whether pipeline should fail or continue based on issues found | boolean | true | true / false | Optional | |
| projectName | Name of project to be associated with the test report | string | | | Optional ||
| organization | id or name of the organisation under which the project is tested| string | 'always' |'always' / 'onIssuesFound' /'never' | Optional | |
| testDirectory | Alternate Working Directory | string | | | Optional | Used to test manifest file from this directory other than root |
| additionalArguments | Additional command line arguements related to command selected | string | | | Optional | Reference <https://docs.snyk.io/snyk-cli/guides-for-our-cli/cli-reference> |
--------------------------------------------------------------------------------------------------------------------------------------------------

## Use Cases

### Snyk Test for Application

  ```yaml
  # azure-pipeline.yml
  resources:
    repositories:
      - repository: Snyk
        type: github
        name: knoldus/azurepipeline.snyk.app #replace with your repo details
        ref: <respective branch name>
        endpoint: 'ADO Connection'

    steps:
    - template: appTest.yml@Snyk
      parameters:        
        nodeVersion: '${{parameters.nodeVersion}}'
        snykServiceConnectionName: '${{parameters.snykServiceConnectionName}}'
        targetFile: '${{parameters.targetFile}}'
        severityThreshold: '${{parameters.severityThreshold}}'
        monitorWhen: '${{parameters.monitorWhen}}'
        failOnIssues: ${{parameters.failOnIssues}}
        projectName: '${{parameters.projectName}}'
        organization: '${{parameters.organisation}}'
        additionalArguments: '${{parameters.additionalArguments}}'
  ```
