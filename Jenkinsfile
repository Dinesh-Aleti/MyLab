pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
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
                nexusArtifactUploader artifacts: [[artifactId: 'DineshDevOpsLab', classifier: '', file: '/var/lib/jenkins/workspace/PipelineJob/target/DineshDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: 'fe5d625d-8b0e-4136-ae5a-51ee4e619e40', groupId: 'com.dineshdevopslab', nexusUrl: '172.20.10.156', nexusVersion: 'nexus3', protocol: 'http', repository: 'DineshDevopsLab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }

        // Stage3 : Deploy
        stage ('Deploy'){
            steps{
                echo 'deploying......'
            }
        }
       

        
    }

}