pipeline {
    agent {
        label 'reactapp-node'
    }
    
    stages {

        stage('Cloneing deployments') {
            steps {
              echo 'Cloneing && pulling....'
              bat 'cd deployment  && git pull origin'
            }
        }
        

        stage('Build') {
            steps {
               bat "docker build --no-cache . -t react-image:v.${BUILD_NUMBER}"
               
            }
        }

         stage('Publish') {
            steps {
               echo 'Publishing....'
               bat "docker tag  react-image:v.${BUILD_NUMBER} react-image:v.${BUILD_NUMBER}"
               bat "docker push react-image:v.${BUILD_NUMBER}"
            }
        }
        stage('Deployment') {
            steps {
               echo 'Deployinging....'
              bat ' kubectl delete -f react-test.yaml'
              bat ' kubectl apply -f react-test.yaml'
            }
        }
    }
    post {
        always {
            echo 'Portal image is deployed in to Kubernetes '
          
        }
        success {
            echo 'Successfully!'
        }
        failure {
            echo 'Failed!'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
