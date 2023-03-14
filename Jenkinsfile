pipeline {
    agent {label 'spc pipeline'}
    triggers {
        pollSCM('17 16 14 3 2') 
    }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/narmadavendi/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('build') {
            steps {
                sh './mvnw package'
            }
        }
        stage ('sonarqube test') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh './mvnw package sonar:sonar'
                }
            }
        }
    }
}