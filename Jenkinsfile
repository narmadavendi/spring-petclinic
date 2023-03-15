pipeline {
    agent {label 'petclinic'}
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
                    sh 'mvn clean package sonar:sonar -Dsonar.login=squ_3a0589dac9e9582389930564adb1913503845152'
                }
            }
        }
        stage ('artifactory configauration') {
            steps {
                rtMavenDeployer (
                    id: "JFROG_DEPLOYER",
                    serverId: "JFROG_SERVER_ID",
                    releaseRepo: LIBS_RELEASE_LOCAL,
                    snapshotRepo: LIBS-SNAPSHOT-LOCAL
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    pom: 'maven-examples/maven-example/pom.xml/',
                    goals: 'clean install',
                    deployerId: "JFROG_DEPLOYER"
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_SERVER_ID" 
                )
            }    
        }
    }
}        