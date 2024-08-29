pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    environment {
        BRANCH_NAME = "${env.BRANCH_NAME}"
        DOCKER_IMAGE = "sachinl:${BUILD_NUMBER}"
        DEPLOY_PORT = getDeployPort(BRANCH_NAME)
    }

    stages {
        stage('Compile and Clean') {
            steps {
                // Display Java version
                sh "java --version"

                // Clean the project
                sh "mvn clean"
            }
        }

        stage('Deploy') {
            steps {
                // Package the application
                sh "mvn package"
            }
        }

        stage('Build Docker image') {
            steps {
                echo "Building Docker image for ${BRANCH_NAME} environment..."
                sh 'ls'
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        // Uncomment the following stages if you want to log in and push the Docker image to a registry

        // stage('Docker Login') {
        //     steps {
        //         withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
        //             sh "docker login -u ashithss -p ${Dockerpwd}"
        //         }
        //     }
        // }

        // stage('Docker Push') {
        //     steps {
        //         sh "docker push ${DOCKER_IMAGE}"
        //     }
        // }

        stage('Docker Deploy') {
            steps {
                echo "Deploying Docker container to port ${DEPLOY_PORT} for ${BRANCH_NAME} environment..."
                sh "docker run -itd -p ${DEPLOY_PORT}:8080 ${DOCKER_IMAGE}"
            }
        }

        stage('Archiving') {
            steps {
                // Archive the built JAR file
                archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

def getDeployPort(String branchName) {
    switch (branchName) {
        case 'dev':
            return "8081"
        case 'QA':
            return "8082"
        case 'Prod':
            return "8083"
        default:
            return "8084"
    }
}

