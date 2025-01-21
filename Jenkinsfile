pipeline {
    agent any
    environment {
        HELM_HOME = '/usr/local/bin'
        KUBECONFIG = '/root/.kube/config'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Helm Chart') {
            steps {
                script {
                    // Read parameters for service name, chart name, and port
                    def serviceName = params.SERVICE_NAME
                    def chartName = params.CHART_NAME
                    def port = params.DB_PORT ?: '5432' // Default port if not provided

                    // Deploy the Helm chart
                    sh """
                    helm upgrade --install ${serviceName} ./charts/${chartName} \
                        --set service.name=${serviceName} \
                        --set service.port=${port}
                    """
                }
            }
        }
    }
    parameters {
        string(name: 'SERVICE_NAME', defaultValue: 'postgres-instance', description: 'Name of the service to deploy')
        string(name: 'CHART_NAME', defaultValue: 'postgres-chart', description: 'Name of the Helm chart to use')
        string(name: 'DB_PORT', defaultValue: '5432', description: 'Port for the service')
    }
}
