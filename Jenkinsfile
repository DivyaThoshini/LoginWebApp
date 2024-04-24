pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
environment {
    NEXUS_VERSION = "nexus3"
    NEXUS_PROTOCOL = "http"
    NEXUS_URL = "192.168.1.128:8081"
    NEXUS_REPOSITORY = "LoginWebApp"
    NEXUS_CREDENTIAL_ID = "	9a50a8ee-76c6-42e0-ae5d-29e55d6eb6bd"
    
}
stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage ('Deployments'){
                stage ('Deploy to Staging Server'){
                    steps {
                        sh "scp **/*.war jenkins@${params.tomcat_stag}:/usr/share/tomcat/webapps"
                    }
                }
            }
        }
    }
}
