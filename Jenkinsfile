pipeline {
    agent any
    
    environment {
        AZURE_CREDENTIALS = credentials('Azure Service Principal')
        AZURE_RESOURCE_GROUP = 'your-resource-group'
        AZURE_APP_SERVICE_NAME = 'your-app-service-name'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout your code repository
                git 'your-repository-url'
            }
        }
        
        stage('Build') {
            steps {
                // Build your code (e.g., Maven, Gradle)
                // Example: sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Azure') {
            steps {
                script {
                    // Login to Azure using Azure CLI
                    withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS, 
                                                            subscriptionId: env.AZURE_SUBSCRIPTION_ID,
                                                            tenantId: env.AZURE_TENANT_ID)]) {
                        sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID'
                    }
                    
                    // Deploy to Azure App Service
                    sh "az webapp deployment source config-zip --resource-group $AZURE_RESOURCE_GROUP --name $AZURE_APP_SERVICE_NAME --src target/*.war"
                }
            }
        }
    }
}
