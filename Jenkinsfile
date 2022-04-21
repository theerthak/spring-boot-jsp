pipeline {
    agent any

    tools {
        maven '3.8.5'
    }
parameters{
 
  string(name: 'SERVER_IP', defaultValue: '34.207.190.115')
}
    
  stages {
        stage('Source') {
            steps {
                git branch: 'main', credentialsId: 'c2d5c505-d1a9-4995-a724-b8d65dab8007', url: 'https://github.com/theerthak/spring-boot-jsp.git'
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
                    ssh root@${SERVER_IP} -p 2232 rm -rf /var/www/html/*
                    rsync -avzP -e 'ssh -p 2232' target/news-${version}.jar root@${SERVER_IP}:/var/www/html/
                    ssh root@${SERVER_IP} -p 2232 cd /var/www/html/
                    pwd
                    java -jar -Dserver.port=8085 news-${version}.jar

                '''
            }
        }
    }
}
