pipeline {
    agent any 
    tools {
        maven 
    
    }
    stages {
        stage('Compile and Clean') { 
            steps {
                // Run Maven on a Unix agent.
                sh "java --version"
                sh "mvn clean"
            }
        }
        stage('deploy') { 
            
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image'){
          
            steps {
                echo "Hello Java Express"
                sh 'ls'
                sh 'docker build -t  ashithss/assignment2:${BUILD_NUMBER} .'
            }
        }
        // stage('Docker Login'){
            
        //     steps {
        //          withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
        //             sh "docker login -u ashithss -p ${Dockerpwd}"
        //         }
        //     }                
        // }
        // stage('Docker Push'){
        //     steps {
        //         sh 'docker push ashithss/assignment2:${BUILD_NUMBER}'
        //     }
        // }
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p  8081:8080 ashithss/assignment2:${BUILD_NUMBER}'
            }
        }
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

