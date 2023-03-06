pipeline {
    agent {
        label "${params.label}"
    }
    triggers{
        pollSCM('* * * * *')
    }
    parameters{
        choice(name: 'Branch_Name', choices: ['main', 'test', 'dev'], description: 'selecting branch to build')
        choice(name: 'label', choices: ['ubuntu1', 'ubuntu2', 'ubuntu3'], description: 'selecting label')
        string(name: 'maven_goal', defaultValue: 'clean', description: 'selecting maven goal')
    }
    stages {
        stage ('vcs') {
            steps {
                git branch: "${params.Branch_Name}",
                    url: 'https://github.com/spring-projects/spring-petclinic.git' 
            }
        }
        stage ('package') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn  ${params.maven_goal} sonar:sonar"
                }
            }
        }
        stage ('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
    }
    post{
        always{ 
            echo "Build is Completed"
            mail to: 'namobuddhay398@gmail.com',
                 body: """Job is Completed for build_number:$env.BUILD_NUMBER
                          Job is completed for job_name:$env.JOB_NAME
                          Job is completed for node_name:$env.NODE_NAME
                        """ ,
                 subject: 'This is about job status'
        }
        failure{
            echo "Build is Failed"
            mail to: 'namobuddhay398@gmail.com',
                 subject: 'This is about job status',
                 body: """Job is Failed for $env.BUILD_NUMBER
                          Job is Failed for $env.JOB_NAME
                          Job is Failed for $env.NODE_NAME
                        """ 
        }
        success{
            echo "Build is Success so testing for junit reports"
            junit '**/surefire-reports/*.xml'
            mail to: 'namobuddhay398@gmail.com',
                 subject: 'This is about job status',
                 body: """Job is Success for $env.BUILD_NUMBER
                          Job is Success for $env.JOB_NAME
                          Job is Success for $env.NODE_NAME
                        """ 
        }
    }
}
