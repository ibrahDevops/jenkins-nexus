pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "54.174.113.149:8081"
        NEXUS_REPOSITORY = "maven-central-repo"
        NEXUS_CREDENTIAL_ID = "NEXUS_CRED"
    }
   stages {
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                       
                        nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            
                            protocol: 'http',
                            
                            nexusUrl: '54.174.113.149:8081',
                            
                            groupId: 'pom.com.mycompany.app',
                            
                            version: 'pom.1.0-SNAPSHOT',
                            
                            repository: 'maven-central-repo',
                            
                            credentialsId: 'NEXUS_ID',
                            
                            artifacts: [
                                
                                [artifactId: 'pom.my-app',
                                
                                classifier: '',
                                
                                file: artifactPath,
                                
                                type: pom.packaging],
                                
                                [artifactId: pom.my-app,
                                
                                classifier: '',
                                
                                file: "pom.xml",
                                
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}
