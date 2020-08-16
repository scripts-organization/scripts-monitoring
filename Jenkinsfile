pipeline {
   agent any

   environment {
     //  You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

     SERVICE_NAME = "scripts-monitoring"     
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh '''echo No build required...monitoring'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'echo No docker image for...monitoring'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                // withKubeConfig(contextName: 'default', credentialsId: '9a91910b-c106-47bc-bc12-757dfd2ad6a2', namespace: 'default', serverUrl: '${KUBERNETES_API_SERVER}') {
                    sh 'envsubst < ${WORKSPACE}/influxdb/influxdb-config.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/influxdb/influxdb-data.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/influxdb/influxdb-deployment.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/influxdb/influxdb-secrets.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/influxdb/influxdb-service.yaml | kubectl apply -f -'

                    sh 'envsubst < ${WORKSPACE}/telegraf/telegraf-config.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/telegraf/telegraf-deployment.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/telegraf-secrets.yaml | kubectl apply -f -'

                    sh 'envsubst < ${WORKSPACE}/grafana/grafana-deployment.yaml | kubectl apply -f -'
                    sh 'envsubst < ${WORKSPACE}/grafana/grafana-service.yaml | kubectl apply -f -'
                    

                // }
          }
      }
   }
}
