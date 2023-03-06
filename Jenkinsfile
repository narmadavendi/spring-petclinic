pipeline {
    agent {
        label 'ubuntu1'
    }
    triggers{
        pollSCM('* * * * *')
    }
    stages {
        stage ('vcs') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/spring-projects/spring-petclinic.git' 
            }
        }
        stage ('package') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
        stage ('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
        stage('junit'){
            steps{
                junit '**/surefire-reports/*.xml'
            }
        }
    }
}
