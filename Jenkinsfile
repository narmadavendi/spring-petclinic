pipeline {
    agent { label 'ubuntu1' }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('package') {
            steps {
			    sh 'export PATH="/usr/lib/jvm/java-17-openjdk-amd64/bin/java/bin:$PATH"',
                sh 'mvn package'
            }
        }
        stage ('post build') {
            archiveArtifacts artifacts: '**/target/*.jar',
                             onlyIfSuccessful: true
            junit testResults: '**/surefire-reports/TEST-*.xml'
        }

    }
}