node {
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository and see if poll scm is working or not
        git 'https://github.com/UditSharma1632/jenkins.git'
        
    }
    stage('Build and Package') {
                sh "mvn clean package"
    }
     
    stage('Nexus') {
        nexusArtifactUploader artifacts: [[artifactId: 'id', classifier: '', file: 'target/jenkins-1.0.0.jar', type: 'jar']], 
            credentialsId: 'Nexus', groupId: 'com.example', nexusUrl: 'host.docker.internal:8110', nexusVersion: 'nexus3', 
            protocol: 'http', repository: 'full-pipeline', version: '0.0.1-SNAPSHOT'
    }
    
    stage('Executing Playbook') { 
                ansiblePlaybook (credentialsId: 'id_rsa', disableHostKeyChecking: true,
                                 installation: 'ansible', inventory: 'inventory.inv', playbook: 'playbook.yml')
        
    }
    
    
}
