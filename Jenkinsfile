pipeline {
    agent ubuntu1
    stages {
        stage ('vcs') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/spring-projects/spring-petclinic.git' 
            }
        stage ('package') {
            steps {
                sh 'export PATH="/usr/lib/jvm/java-17-openjdk-amd64',
                sh './mvnw package'
            }
        stage ('post build') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}