pipeline {
  agent any
  stages {
    stage('Git-checkout') { // for display purpose
      when {
        not {
          branch 'master'
        }
      }
      steps {
        git branch: env.BRANCH_NAME,
          url: 'https://github.com/UditSharma1632/pipeline.git'
      }
    }

    stage('Build') {
//       when {
//         anyOf {
//           branch 'env.BRANCH_NAME/*'
//         }
//       }
      steps {
        sh "mvn clean test"
      }
    }

    stage('Package') {
//       when {
//         anyOf {
//           branch 'env.BRANCH_NAME/*'
//         }
//       }
      steps {
        sh "mvn package"
      }
    }

    stage('Nexus') {
//       when {
//         anyOf {
//           branch 'env.BRANCH_NAME/*'
//         }
//       }
      steps {
        nexusArtifactUploader artifacts: [
            [artifactId: 'demo',
              classifier: '', file: 'target/demo-1.0.0.jar',
              type: 'jar'
            ]
          ], credentialsId: 'nexus3', groupId: 'com.example',
          nexusUrl: 'host.docker.internal:8110', nexusVersion: 'nexus3',
          protocol: 'http',
          repository: 'snapshot',
          version: '1.0.0'
      }
    }

    stage('ansible-deploy') {
      when {
        anyOf {
          branch 'env.BRANCH_NAME/*'
        }
      }
      steps {
        ansiblePlaybook(credentialsId: 'id_rsa', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory.inv', playbook: 'plabook.yml')
      }
    }
  }
}
