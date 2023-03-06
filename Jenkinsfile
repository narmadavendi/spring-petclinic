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
                sh """export PATH=/usr/lib/jvm/java-17-openjdk-amd64/bin:$PATH
                      mvn clean package sonar:sonar
                    """  
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
