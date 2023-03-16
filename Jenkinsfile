pipeline {
    agent {label 'narmada'}
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
        stage ('sonarqube tests') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean package sonar:sonar '
                }
            }
        }
        stage ('Artifactory configauration') {
            steps {
                rtMavenDeployer (
                    id: "jfrog-deployer-id",
                    serverId: "jfrog-server-id",
                    releaseRepo: 'libs_release_local',
                    snapshotRepo: 'libs_snapshot_local'
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'MAVEN',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "jfrog-deployer-id"
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrog-server-id"
                )
            }
        }        
    }
}        