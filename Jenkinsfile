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
                script {

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"     

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '72f01522-fe8e-44fd-93d7-673ce646516e', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.156:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
                }
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

        Stage 5 : Deploy

        stage ('Deploy to Tomcat'){
            steps {
                echo "Deploying ...."
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                            cleanRemote:false,
                            execCommand: 'ansible-playbook /opt/playbooks/installanddeploy.yaml -i /opt/playbooks/hosts',
                            execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        }

        
    }

}