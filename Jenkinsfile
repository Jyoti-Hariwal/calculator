pipeline {
  environment {
    registry = "riteshk0398/calculator"
    registryCredential = 'dockerhub'
    KUBECONFIG="$JENKINS_HOME/.kube/config2"
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        sh "printenv"
        
        sh "docker build -t jyoti26/calculator:$BUILD_ID-$BRANCH_NAME ." 
        //sh "docker run -dp 80:80 jyoti26/calculator:$BUILD_ID-$BRANCH_NAME"
        sh "docker push jyoti26/calculator:$BUILD_ID-$BRANCH_NAME"
      }
    }
    stage('Creating Deployment') {
      steps {
    
        sh  '''#!/bin/bash
                
                if [[ $GIT_BRANCH == "development" ]]
                then
                    kubectl set image deployment/aes-app httpd=jyoti26/calculator:$BUILD_ID-$BRANCH_NAME -n $BRANCH_NAME
                elif [[ $GIT_BRANCH == "master" ]]
                then
                    kubectl set image deployment/aes-app httpd=jyoti26/calculator:$BUILD_ID-$BRANCH_NAME -n production
                fi         
            '''
      }
    }

    stage('') {
      steps{
        sh '''
            
            kubectl get svc -n $BRANCH_NAME
            '''
      }
    }
    }
}
