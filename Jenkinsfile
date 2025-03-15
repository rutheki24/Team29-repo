pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-amazon-corretto.x86_64"
        MAVEN_HOME = "/usr/share/maven"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rutheki24/Team29-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn sonar:sonar -Dsonar.projectKey=Team29-repo'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn test'
            }
        }


        stage('Deploy via SCP') {
            steps {
                sh '''
                scp target/*.war ec2-user@54.242.131.133:/var/lib/tomcat/webapps/
                ssh ec2-user@54.242.131.133 "sudo systemctl restart tomcat"
                '''
            }
        }
    }
}
