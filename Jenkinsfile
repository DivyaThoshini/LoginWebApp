pipeline {
    agent any
   
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://192.168.1.128:8081"
        NEXUS_REPOSITORY = "LoginWebApp"
        NEXUS_CREDENTIAL_ID = "9a50a8ee-76c6-42e0-ae5d-29e55d6eb6bd" 
    }
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git "https://github.com/DivyaThoshini/LoginWebApp.git"
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Publish to Nexus') {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml"
                    fileMyGlob = findFiles(glob: "target/*.${pom.packaging}")
                    artifactPath = fileMyGlob[0].path
                    artifactExists = fileExists artifactPath
                    if (artifactExists) {
                        echo "File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version: ${pom.version}"
                        nexusArtifactUploader(
                            nexusVersion: "${NEXUS_VERSION}",
                            protocol: "${NEXUS_PROTOCOL}",
                            nexusUrl: "${NEXUS_URL}",
                            groupId: "${pom.groupId}",
                            version: "${pom.version}",
                            repository: "${NEXUS_REPOSITORY}",
                            credentialsId: "${NEXUS_CREDENTIAL_ID}",
                            artifacts: [
                                [
                                    artifactId: "${pom.artifactId}",
                                    classifier: '',
                                    file: artifactPath,
                                    type: "${pom.packaging}"
                                ]
                            ]
                    )

                    } else {
                        echo "Error: Artifact not found"
                    }
                }
            }
        }
    }
}
