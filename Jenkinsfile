pipeline {
    agent none
    triggers{
        pollSCM('* * * * *')
    }
    parameters{
        choice(name: 'Branch_Name', choices: ['main', 'test', 'dev'], description: 'selecting branch to build')
        /* choice(name: 'label', choices: ['ubuntu1', 'ubuntu2', 'ubuntu3'], description: 'selecting label')*/
        string(name: 'maven_goal', defaultValue: 'clean', description: 'selecting maven goal')
    }
    stages {
        stage ('vcs') {
            agent{
               label 'ubuntu1'
            }
            steps {
                git branch: "${params.Branch_Name}",
                    url: 'https://github.com/spring-projects/spring-petclinic.git' 
            }
        }
        stage ('package') {
            agent{
               label 'ubuntu1'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn  ${params.maven_goal} sonar:sonar"
                }
            }
        }
        stage ('post build') {
            agent{
                label 'ubuntu1'
            }
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
        stage ('Artifactory configuration') {
            agent{
                label 'ubuntu1'
            }
            steps {
                    rtMavenDeployer (
                    id: "jfrog-deployer-id",
                    serverId: "jfrog-server-id",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )
            }
        }
        stage ('Exec Maven') {
            agent{
                label 'ubuntu1'
            }
            steps {
                rtMavenRun (
                    tool: 'maven', 
                    pom: 'pom.xml',
                    goals: "${params.maven_goal}",
                    deployerId: "jfrog-deployer-id"
                )
            }
        }
        stage ('Publish build info') {
            agent{
                label 'ubuntu1'
            }
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrog-server-id"
                )
            } 
        }
        stage('deploy'){
            agent{
                label 'ansible'
            }
            steps{
                sh """
                      ansible -i ./ansible/hosts.yaml -m ping all
                      ansible-playbook -i ./ansible/hosts.yaml ./ansible/spc.yaml
                    """  
            }
        }
    }
    post{
        always{ 
            echo "Build is Completed"
            mail to: 'namobuddhay398@gmail.com',
                 body: """Job is Completed for build_number:$env.BUILD_NUMBER
                          \n Job is completed for job_name:$env.JOB_NAME
                          \n Job is completed for node_name:$env.NODE_NAME
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
