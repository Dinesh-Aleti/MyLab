pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        GroupId = readMavenPom().getGroupId()
        Name = readMavenPom().getName()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // stage3: Publish artifacts to Nexus
        stage('Publish to Nexus'){
            steps{
                nexusArtifactUploader artifacts: 
                [[artifactId: '${ArtifactId}', 
                classifier: '', 
                file: 'target/VinayDevopsLab-0.0.4-SNAPSHOT.war', 
                type: 'war']], 
                credentialsId: '72f01522-fe8e-44fd-93d7-673ce646516e', 
                groupId: '${GroupId}', 
                nexusUrl: '172.20.10.156:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'VinaysDevOpsLab-SNAPSHOT', 
                version: '${Version}'
            }
        }

        // stage 4: Print some information

        stage('Print Environment variables'){
            steps{
                echo "ArtifactId is '${ArtifactId}'"
                echo "VersionId is '${Version}'"
                echo "GroupId is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }

        // Stage 5 : Deploy
        stage ('Deploy'){
            steps{
                echo 'deploying......'
            }
        }
       

        
    }

}