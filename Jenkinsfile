pipeline {
    agent any 

    environment {
        AZURE_RG_NAME = {'Demo-RG-3'}','{'Demo-RG-2'}
        AZURE_LOCATION = 'EastUS'
        AZURE_TEMPLATE_FILE = '${AZURE_TEMPLATE_FILE}'
        AZURE_PARAMETERS_FILE = '${AZURE_PARAMETERS_FILE}'
        //AZURE_DEPLOYMENT_NAME = 'Demo'
        AZURE_CLIENT_ID = '${AZURE_CLIENT_ID}'
        AZURE_CLIENT_SECRET = '${AZURE_CLIENT_SECRET}'
        AZURE_TENANT_ID = '${AZURE_TENANT_ID}'
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
                    //sh "az group create --name \$AZURE_RG_NAME --location \$AZURE_LOCATION"
                    sh "az group delete --resource-group \$AZURE_RG_NAME --yes"
                    //sh "az deployment group create --resource-group \$AZURE_RG_NAME --template-file \$AZURE_TEMPLATE_FILE --parameters \$AZURE_PARAMETERS_FILE --name \$AZURE_DEPLOYMENT_NAME"
                }
            }
        }
    }
}
