pipeline {
    agent {label 'spe pipeline'}
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
    }
}