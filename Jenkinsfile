pipeline {
    agent {label 'spc pipeline'}
    triggers {
        pollSCM('* * * * *') 
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
                    sh 'mvn clean package sonar:sonar -Dsonar.projectkey=narmada'
                }
            }
        }
    }
}