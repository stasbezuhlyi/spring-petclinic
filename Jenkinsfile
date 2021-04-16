pipeline {
    agent any
    environment {
        IMAGE_BASE = 'rodley/spring-petclinic'
        IMAGE_TAG = "$BUILD_NUMBER"
        IMAGE_NAME = "${env.IMAGE_BASE}:${env.IMAGE_TAG}"
        IMAGE_NAME_LATEST = "${env.IMAGE_BASE}:latest"
        DOCKERFILE_NAME = "Dockerfile"
}

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Running build automation'
                sh '''
                    ./mvnw package
                   '''
           }
        }

    stage('Create and push image') {
       steps {
        script {
          def dockerImage = docker.build("${env.IMAGE_NAME}", "-f ${env.DOCKERFILE_NAME} .")
           echo "Created Docker Image ${env.IMAGE_NAME}" 
           docker.withRegistry('', 'dockerhub_id') {
           dockerImage.push()
           dockerImage.push("latest")
      }
      echo "Pushed Docker Image: ${env.IMAGE_NAME}"
    }
    sh "docker rmi ${env.IMAGE_NAME} ${env.IMAGE_NAME_LATEST}"
  }
}
        
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
