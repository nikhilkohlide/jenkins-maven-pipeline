pipeline {
    agent any

    tools {
        jdk 'jdk-17'
        maven 'Maven-3.9.6'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/nikhilkohlide/jenkins-maven-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh '''
                    # Example: Run the jar after building
                    pkill -f "java -jar" || true
                    nohup java -jar target/*.jar > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
    }
}

