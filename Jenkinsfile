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

        // stage3: Publish artifacts to Nexus   //This is done with the help of nexus artifact uploader     (pipeline sysntax)

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

        //Stage 5 : Deploy // This is done with the help of push over ssh plugin (ssh:publisher send build artifacts over ssh)

        // stage('Deploy to Tomcat'){ 
        //     steps {
        //         echo "Deploying ...."
        //         sshPublisher(publishers: 
        //         [sshPublisherDesc(
        //             configName: 'Ansible_Controller', 
        //             transfers: [
        //                 sshTransfer(
        //                     cleanRemote: false,
        //                     execCommand: 'ansible-playbook /opt/playbooks/installanddeploy.yaml -i /opt/playbooks/hosts',
        //                     execTimeout: 120000
        //                 )
        //             ], 
        //             usePromotionTimestamp: false, 
        //             useWorkspaceInPromotion: false, 
        //             verbose: false)
        //             ])
            
        //     }
        // }
        
        //Stage 6 : Deploy the build artifact to Docker

        stage('Deploy to Docker'){ 
            steps {
                echo "Deploying ...."
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false,
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts',
                            execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        }

        //Stage 6 : Publish the source code to Sonarqube

        stage('Sonarqube Analysis'){
            steps{
                echo 'Source code published to sonarqube for SCA'
                withSonarQubeEnv('sonarqube'){
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
    }

}