pipeline {
    agent any

    tools {
        jdk 'JDK'        // Use your configured JDK name in Jenkins
        maven 'Maven'     // Use your configured Maven installation name
    }

    environment {
        // optional custom env vars
        APP_NAME = "Play-with-Jenkins"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Fetching source code from GitHub...'
                // Jenkins automatically knows your repo if using "Pipeline script from SCM"
                // Or you can specify manually like below:
                git branch: 'main',
                    url: 'https://github.com/raveendra11/Play-with-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Maven project...'
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running JUnit tests...'
                bat 'mvn test'
            }
            post {
                always {
                    echo 'Publishing test results...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the Spring Boot JAR...'
                bat 'mvn package -DskipTests'
            }
        }

        stage('Run Application') {
    steps {
        echo 'Running Spring Boot App on port 8090...'
        bat '''
        for /f %%i in ('dir /b target\\*.jar') do set JAR_NAME=%%i
        echo Found jar: %JAR_NAME%
        java -jar target\\%JAR_NAME% --server.port=8090
        '''
    }
}


    }

    post {
        success {
            echo ' Build and tests succeeded!'
        }
        failure {
            echo ' Build failed. Check logs for details.'
        }
    }
}
