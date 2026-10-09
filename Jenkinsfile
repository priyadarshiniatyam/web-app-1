
pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://localhost:9090/manager/text'
        WAR_FILE = 'target/demo-0.0.1-SNAPSHOT.war'
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/priyadarshiniatyam/web-app-1.git',
                    branch: 'main'
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw.cmd clean package'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'tomcat-credentials',
                    usernameVariable: 'TOMCAT_USER',
                    passwordVariable: 'TOMCAT_PASS'
                )]) {
                    bat '''
                        curl --fail --silent --show-error ^
                        -u "%TOMCAT_USER%:%TOMCAT_PASS%" ^
                        --upload-file "%WAR_FILE%" ^
                        "%TOMCAT_URL%/deploy?path=/springbootapp1&update=true"
                    '''
                }
            }
        }
    }
}
