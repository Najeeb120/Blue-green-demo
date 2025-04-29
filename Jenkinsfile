pipeline {

  agent any

 parameters {
    stringI(name: 'IMAGE_NAME', defaultValue: 'dockerimage name',)
    string(name: 'NAMESPACE', defaultValue: 'hello-world-app')
    string(name: 'DEPLOY_DIR', defaultValue: 'kubernetes')
    string(name: 'APP_NAME',)defaultValue: 'hello-app')
    choice(name: 'VERSION_NAME', choices: ['blue','green'])
  }

  environment {
    IMAGE_TAG = "${BUILD_NUMBER}"
  }

  stages{
    stage('Clone repository')
    steps{
        git  'https://github.com/docker-python-helloworld.git' 

    }

    stage('Build Docker Image')
    {
        steps {
            script {
                docker.build("${params.IMAGE_NAME}:{env.IMAGE_TAG}")
            }
        }
    }
    

    stage('Push Docker Image'){
        steps {
            withCredentials([usernamePassword(credentialsID: 'dockerhub-credentials',usernameVariable: 'DOCKER_USER',passwordVariable: 'DOCKER_PASS')])
               sh '''
                  echo "DOCKER_PASS" | docker login -u "$DOCKER_USER"  --password-stdin
                  docker push ${IMAGE_NAME}:{IMAGE_TAG}
                  '''
            }

        }

    stage('Deploy Target Version'){
        steps {
          script {
            def deploySample = readFile("$params.K8S_DEPLOY_DIR}/${params.VERSION_NAME} --deployment.yaml")
            def modifiedYaml = deploySample.replace('IMAGE_TAG',env.IMAGE_TAG)
            writeFile file: "rendered=${params.TARGET_VERSION}.yaml", text: renderedYaml
          }
          sh "kubectl apply -n ${params.NAMESPACE} -f rendered-${params.VERSION_NAME}.yaml"
        }
    }

   stage('Testing the deployment'){
        steps {
          sh '''
          echo " wait for new version to complete ...."
          kubectl rollout status deployment/${APP_NAME}-${VERSION_NAME} -n ${NAMESPACE}
          '''
        }
    }


  }

}