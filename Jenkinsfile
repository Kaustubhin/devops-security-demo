pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }
      stage('Unit Tests') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }
      stage ('K8s-Deployment') {
        steps{
          withKubeConfig([credentialsId:'c3c49f29-46a7-4093-849c-a2ed24acf901']) {
            sh "sed -i 's#replace#kaustubhin/numeric-app:1.0.0#g' k8s_deployment_service.yaml"
            sh "kubectl apply -f k8s_deployment_service.yaml"
          }
        }
      }      
    }


}