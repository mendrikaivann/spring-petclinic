pipeline {
    agent any
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}
