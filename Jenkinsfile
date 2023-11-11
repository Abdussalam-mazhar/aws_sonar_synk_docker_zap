pipeline {
  agent any
  tools { 
        maven 'Maven_3.5.2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=salam_salam -Dsonar.organization=salam -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=75d881a4f045509b9e02e232c60a4f8e42ced785'			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SYNK_TOKEN', variable: 'SYNK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "Docker_cred", url: ""]) {
                 script{
                 app =  docker.build("newworld")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('132142301041.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:ecr-user') {
                    app.push("latest")
                    }
                }
            }
    	}
	   
//	stage('Kubernetes Deployment of ASG Bugg Web Application') {
//	   steps {
//	      withKubeConfig([credentialsId: 'kubelogin']) {
///		  sh('kubectl delete all --all -n devsecops')
//		  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
//		}
//	      }
 //  	}
	   
//	stage ('wait_for_testing'){
//	   steps {
//		   sh 'pwd; sleep 180; echo "Application Has been deployed on K8S"'
//	   	}
//	   }
	   
//	stage('RunDASTUsingZAP') {
 //         steps {
//		    withKubeConfig([credentialsId: 'kubelogin']) {
//				sh('zap.sh -cmd -quickurl http://$(kubectl get services/asgbuggy --namespace=devsecops -o json| jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
//				archiveArtifacts artifacts: 'zap_report.html'
//		    }
//	     }
 //      } 
  }
}
