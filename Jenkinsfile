node {
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository and see if poll scm is working or not
        git 'https://github.com/UditSharma1632/pipeline.git'
        
    }
    stage('Build and Package') {
                sh "mvn clean package"
    }
     
    stage('Nexus') {
        nexusArtifactUploader artifacts: [[artifactId: 'demo', classifier: '', file: 'target/demo-0.0.1.jar', type: 'jar']],
            credentialsId: 'nexus3', groupId: 'com.example', nexusUrl: 'host.docker.internal:8110', 
            nexusVersion: 'nexus3', protocol: 'http', repository: 'full-pipeline', version: '0.0.1'
    }
    
    
    stage('Executing Playbook') { 
                ansiblePlaybook (credentialsId: 'id_rsa', disableHostKeyChecking: true,
                                 installation: 'ansible', inventory: 'inventory.inv', playbook: 'playbook.yml')
        
    }
    
    
}

