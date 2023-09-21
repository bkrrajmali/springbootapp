pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/malibalakrishna/springbootapp.git']]])     
            }
        }
      stage ('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
  //  stage('Unit Test') {
  //     steps {
  //       echo '<--------------- Unit Testing started  --------------->'
  //       sh 'mvn surefire-report:report'
  //       echo '<------------- Unit Testing stopped  --------------->'
  //     }
  //   }

  //   stage('Sonar Analysis') {
  //     environment {
  //       scannerHome = tool 'sonar-scanner'
  //     }
  //     steps {
  //       echo '<--------------- Sonar Analysis started  --------------->'
  //       //         withSonarQubeEnv('sonar-cloud') {
  //       //         sh "${scannerHome}/bin/sonar-scanner"

  //       // }
  //       withSonarQubeEnv('sonar-cloud') {
  //         sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=springbootapp -Dsonar.organization=malibalakrishna -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2aeb5370772ef98991c02f240fb5a3b38b79c613'
  //         echo '<--------------- Sonar Analysis stopped  --------------->'
  //       }
  //     }
  //   }
  //    stage('Quality Gate') {
  //     steps {
  //       script {
  //         echo '<--------------- Quality Gate started  --------------->'
  //         timeout(time: 1, unit: 'MINUTES') {
  //           def qg = waitForQualityGate()
  //           if (qg.status != 'OK') {
  //             error 'Pipeline failed due to the Quality gate issue'
  //           }
  //         }
  //         echo '<--------------- Quality Gate stopped  --------------->'
  //       }
  //     }
  //   }  
    //Building Docker images
    stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t myrepo .'
                }
            }
    }


stage('Upload to Nexus') {
    steps {
        script {
            nexusArtifactUploader artifacts: [
                // Define the artifacts you want to upload to Nexus
                [artifactId: 'springbootApp', file: 'Springbootapp/target/springbootApp.jar', type: 'jar']
                // Add more artifacts if needed
            ],
            credentialsId: 'nexus',
            groupId: 'com.tcs.angularjs',
            nexusUrl: '18.206.212.197:8081',
            nexusVersion: '3', // Adjust this to your Nexus version (2 or 3)
            protocol: 'http', // Use 'https' if your Nexus server uses HTTPS
            repository: 'maven3',
            version: '1.0.0' // Replace with your desired version
        }
    }
}
  //  stage('Upload to Nexus') {
  //     steps {
  //       script {
  //          echo '<--------------- War Publish Started --------------->'
  //         nexusArtifactUploader artifacts: [
  //           [artifactId: 'springbootApp', classifier: '', file: 'Springbootapp/target/springbootApp.jar', type: 'jar']
  //           ], credentialsId: 'nexus', groupId: 'com.tcs.angularjs', nexusUrl: '18.206.212.197:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven3', version: '1.0'
  //           echo '<--------------- War Publish Ended --------------->'  
          
  //       }
  //     }
	// }
    // // Uploading Docker images into AWS ECR
    // stage('Pushing to ECR') {
    //  steps{  
    //      script {
    //             sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 923770093922.dkr.ecr.us-east-1.amazonaws.com'
    //             sh 'docker tag myrepo:latest 923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
    //             sh 'docker push 923770093922.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
    //      }
    //     }
    //   }

    //    stage('Deploy to EKS') {
    //         steps {
    //             script {
    //                 // Authenticate with the EKS cluster (ensure AWS credentials are configured)
    //                 sh 'aws eks --region us-east-1 update-kubeconfig --name demo-eks'
                    
    //                 // Apply Kubernetes manifest files to deploy your application
    //                   sh "kubectl delete -f eks-deploy-k8s.yaml"
    //                   sh "kubectl apply -f eks-deploy-k8s.yaml"
    //             }
    //         }
    //     }
       
    }
}