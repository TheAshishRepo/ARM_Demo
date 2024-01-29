pipeline {
    agent any

    environment {
        AZURE_RG_NAME = 'Demo-RG'
        AZURE_LOCATION = 'East US'
        AZURE_TEMPLATE_FILE = '${env.WORKSPACE}/arm-template.json'
        AZURE_PARAMETERS_FILE = '${env.WORKSPACE}/arm-parameters.json'
        AZURE_DEPLOYMENT_NAME = 'Demo'
        AZURE_CLIENT_ID = 'd4689707-a743-4d6b-bbc8-d55a481be5ec'
        AZURE_CLIENT_SECRET = 'TWo8Q~lmyTtrugz1M6Ylii_JkcAd7G9sW2Y5Ma9O'
        AZURE_TENANT_ID = 'e3630473-3199-4091-8f2d-bfc939c7a8f0'
        AZURE_SUBSCRIPTION_ID = 'f0c38c3b-77fd-4828-bb35-c7c22eecb247'

    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout your source code repository
                    checkout scm
                }
            }
        }

        stage('Deploy ARM Template') {
            steps {
                script {
                    // Login to Azure using Azure CLI
                    withCredentials([azureServicePrincipal('Jenkins-Azure-Integration-SPN')]) {
                        sh "az login --service-principal -u \$AZURE_CLIENT_ID -p \$AZURE_CLIENT_SECRET --tenant \$AZURE_TENANT_ID"
                    }

                    // Set Azure subscription
                    sh "az account set --subscription \$AZURE_SUBSCRIPTION_ID"

                    // Create or update resource group and deploy ARM template
                    sh "az group create --name \$AZURE_RG_NAME --location \$AZURE_LOCATION"
                    sh "az deployment group create --resource-group \$AZURE_RG_NAME --template-file \$AZURE_TEMPLATE_FILE --parameters \$AZURE_PARAMETERS_FILE --name \$AZURE_DEPLOYMENT_NAME"
                }
            }
        }
    }
}
