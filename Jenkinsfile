pipeline {
    agent any

    tools {
        maven 'maven'
    }
parameters{
 
  string(name: 'SERVER_IP', defaultValue: '65.0.85.217')
}
    
  stages {
        stage('Source') {
            steps {
                 git branch: 'main', credentialsId: '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', url: 'https://github.com/theerthak/spring-boot-jsp.git'

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
                    ssh -p 22 -o StrictHostKeyChecking=no root@${SERVER_IP} -p 22 rm -rf /var/www/html/*
                    rsync -avzP -e 'ssh -p 22' target/news-${version}.jar root@${SERVER_IP}:/var/www/html/
                    ssh root@${SERVER_IP} -p 22 java -jar -Dserver.port=8085 /var/www/html/news-${version}.jar

                '''
            }
        }
    }
}
