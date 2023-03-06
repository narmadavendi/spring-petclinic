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
    post{
        always{
            echo "Build is Completed"
            mail to: 'namobuddhay398@gmail.com'
                 body: 'This is about job status'
                 subject: """
                             Job is Completed for build_number:$env.BUILD_NUMBER
                             Job is completed for job_name:$env.JOB_NAME
                             Job is completed for node_name:$env.NODE_NAME
                            """ 
        }
        failure{
            echo "Build is Failed"
            mail to: 'namobuddhay398@gmail.com'
                 body: 'This is about job status'
                 subject: """
                             Job is Failed for $env.BUILD_NUMBER
                             Job is Failed for $env.JOB_NAME
                             Job is Failed for $env.NODE_NAME
                            """ 
        }
        success{
            echo "Build is Success"
            mail to: 'namobuddhay398@gmail.com'
                 body: 'This is about job status'
                 subject: """
                             Job is Success for $env.BUILD_NUMBER
                             Job is Success for $env.JOB_NAME
                             Job is Success for $env.NODE_NAME
                            """ 
        }
    }
}
