pipeline {
    agent any
    stages {
        stage('Build') {
           steps {
               echo 'Build Process Started'
               sh './gradlew build --no-daemon'
               archiveArtifacts artifacts: 'dist/trainSchedule.zip'
           }
        }
        stage('Docker Build') {
           when {
               branch 'master'
           }
           steps {
               script {
                   customImage = docker.build("gurugreen:${env.BUILD_ID}")
                   customImage.inside {
                       sh 'echo $(curl localhost:8080)'
                   }

               }
           }
        }
        stage('Example Build') {
           when {
               branch 'master'
           }
           steps {
				script {
				    withDockerRegistry('https://registry.hub.docker.com','docker_hub_login'){
				        customImage.push("gurugreen:${env.BUILD_ID}")
				        customImage.push("gurugreen:latest")
				    }

				}
               	}

           }
        }
    }

