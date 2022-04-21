pipeline {
    agent any

    tools {
        maven '3.8.5'
    }
    
  stages {
        stage('Source') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'github', poll: false, url: 'https://github.com/ajilraju/spring-boot-jsp.git'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Copying Artifcats') {
            steps {
                sh '''
                    version=$(perl -nle 'print "$1" if /<version>(v\\d+\\.\\d+\\.\\d+)<\\/version>/' pom.xml)
                    rsync -avzP -e 'ssh -p 2232' target/news-${version}.jar root@${SERVER_IP}:/opt/
                '''
            }
        }
    }
}
