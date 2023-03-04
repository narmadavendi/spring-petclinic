pipeline {
    agent { label 'ubuntu1' }
    tools { jdk 'jdk_17'}
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('package') {
            steps {
                sh './mvnw package'
            }
        }
        stage ('post build') {
            steps { 
                archiveArtifacts artifacts: '**/target/*.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
        }

    }
}
}