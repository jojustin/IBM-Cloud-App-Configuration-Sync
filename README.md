# IBM Cloud App Configuration Sync

This action syncs configuration files in the repository to an IBM Cloud App Configuration instance.  This action automatically updates an IBM Cloud App Configuration instance when the changes are made through the GitHub workflows.

.json files are supported as configuration files. You can use IBM Cloud App Configuration CLI to export the configuration from an existing instance.  Refer https://cloud.ibm.com/docs/app-configuration?topic=app-configuration-cli-plugin-app-configuration-cli#ac-ibmcloud-ac-export for details.

To start using this GitHub action, go to your repository and click the "Actions" tab. This GitHub action will be available from the marketplace under the name "IBM Cloud App Configuration Sync". 


## Secrets 

IBM Cloud API Key & App Configuration Instance GUID should be stored as [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) in the names IBM_CLOUD_API_KEY & IBM_CLOUD_AC_INSTANCE_ID respectively.

## Input parameters

| Input parameter      | Description | Required     |
| :---        |    :----:   |          ---: |
| IBM_CLOUD_API_KEY | IBM Cloud Api key for account login   | Yes |
| AC_INSTANCE_ID | IBM Cloud App Configuration Instance guid | Yes |
| IBM_CLOUD_REGION | IBM Cloud App Configuration Instance region | Yes |
| IBM_CLOUD_RESOURCE_GROUP | IBM Cloud App Configuration Instance resource group | Yes |
| CONFIG_FILE_NAME | Configuration file in the repository  | Yes |
| GITHUB_REPO | Value should be ${{ github.repository }}  | Yes |
| GITHUB_PULL_NUMBER | Value should be ${{ github.event.pull_request.number }}  | Yes |


## Usage example

The following example updates the App Configuration instance when pull requests are merged with changes to appconfig.json.


```
# File: .github/workflows/appconfigsync.yml

name: IBM Cloud App Configuration Sync
author: Josephine Eskaline Joyce
description: workflow to import configurations to IBM Cloud App Configuration service

# Controls when the workflow will run
# Controls when the workflow will run
on:
  # Triggers the workflow on pull requests are approved
  pull_request:
    branches: [ main ]
    types: [ closed ]

jobs:
  updateConfig:
    runs-on: ubuntu-latest

    steps:
      - uses: jojustin/IBM-Cloud-App-Configuration-Syncv0.1.0
        with:
          IBM_CLOUD_API_KEY: ${{ secrets.IBM_CLOUD_API_KEY }}
          AC_INSTANCE_ID: ${{ secrets.IBM_CLOUD_AC_INSTANCE_ID }}
          IBM_CLOUD_REGION: au-syd
          IBM_CLOUD_RESOURCE_GROUP: default
          CONFIG_FILE_NAME: 'appconfig.json'
          GITHUB_REPO: ${{ github.repository }}
          GITHUB_PULL_NUMBER: ${{ github.event.pull_request.number }}
```

