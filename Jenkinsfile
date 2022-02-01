pipeline {
  agent any
  environment {
    VERSION = readMavenPom().getVersion()
  }

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
        echo "${env.BRANCH_NAME}"
      }
    }

    stage('Build') {
//             when {
//               anyOf {
//                 branch 'env.BRANCH_NAME/*'
//               }
//             }
      steps {
        sh "mvn clean test"
      }
    }

    stage('Package') {
//             when {
//               anyOf {
//                 branch 'env.BRANCH_NAME/*'
//               }
//             }
      steps {
        sh "mvn package"
      }
    }

    stage('Nexus') {
//             when {
//               anyOf {
//                 branch 'env.BRANCH_NAME/*'
//               }
//             }
      steps {
        script {
          if (env.BRANCH_NAME == 'develop') {
            echo 'I only execute on the master branch'

            nexusArtifactUploader artifacts: [
                [artifactId: 'demo',
                  classifier: '', file: 'target/demo-0.0.1-SNAPSHOT.jar',
                  type: 'jar'
                ]
              ], credentialsId: 'nexus3', groupId: 'com.example',
              nexusUrl: 'localhost:8110', nexusVersion: 'nexus3',
              protocol: 'http',
              repository: 'snapshot',
              version: "${VERSION}"
          } else {
            if (env.BRANCH_NAME == 'release') {
              echo 'I execute elsewhere'
              nexusArtifactUploader artifacts: [
                  [artifactId: 'demo',
                    classifier: '', file: 'target/demo-0.0.1-RELEASE.jar',
                    type: 'jar'
                  ]
                ], credentialsId: 'nexus3', groupId: 'com.example',
                nexusUrl: 'localhost:8110', nexusVersion: 'nexus3',
                protocol: 'http',
                repository: 'release',
                version: "${VERSION}"
            } else {
              nexusArtifactUploader artifacts: [
                  [artifactId: 'demo',
                    classifier: '', file: 'target/demo-0.0.1-HOTFIX.jar',
                    type: 'jar'
                  ]
                ], credentialsId: 'nexus3', groupId: 'com.example',
                nexusUrl: 'localhost:8110', nexusVersion: 'nexus3',
                protocol: 'http',
                repository: 'hotfix',
                version: "${VERSION}"
            }
          }
        }
      }
    }

  stage('ansible-deploy') {
//       when {
//         not {
//           branch 'master'
//         }
//       }
      steps {
        script {
          if (env.BRANCH_NAME == 'develop') {
            ansiblePlaybook(
              disableHostKeyChecking: true,
              installation: 'ansible',
              inventory: 'inventory/inventory.inv',
              playbook: 'playbook.yml',
              extras: '-e target_environment=dev' )
        }
        else{
if (env.BRANCH_NAME == 'hotfix') {
            ansiblePlaybook(
              disableHostKeyChecking: true,
              installation: 'ansible',
              inventory: 'inventory/inventory.inv',
              playbook: 'playbook.yml',
              extras: '-e target_environment=hotfix' )
        }
        else{
         ansiblePlaybook(
              disableHostKeyChecking: true,
              installation: 'ansible',
              inventory: 'inventory/inventory.inv',
              playbook: 'playbook.yml',
              extras: '-e target_environment=release' ) 
        }
          
        }
      }
    }
  }
}
}
