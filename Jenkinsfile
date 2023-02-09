pipeline{
    
    agent any 

    parameters{

        choice(name: 'action', choices: 'apply\ndelete', description: 'apply or delete the application files on eks cluster')
        string(name: 'cluster', defaultValue: 'demo-cluster', description: 'Choose EKS Cluster Nane')
        string(name: 'region', defaultValue: 'us-east-1', description: 'Choose AWS region of cluster')
        string(name: 'AWS_Account_ID', defaultValue: '496157679619', description: 'Choose AWS Account ID ')
    }
    environment{

        ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        SECRET_KEY = credentials('AWS_SECRET_KEY_ID')
    }
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                     git branch: 'main', url: 'https://github.com/vikash-kumar01/demoproj.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        // stage('Static code analysis'){
            
        //     steps{
                
        //         script{
                    
        //             withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
        //                 sh 'mvn clean package sonar:sonar'
        //             }
        //            }
                    
        //         }
        //     }
        //     stage('Quality Gate Status'){
                
        //         steps{
                    
        //             script{
                        
        //                 waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
        //             }
        //         }
        //     }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
            stage('docker image building and tagging'){

                steps{

                    script{

                        sh 'docker build -t testmyapp .'
                        sh 'docker tag testmyapp:latest 496157679619.dkr.ecr.us-east-1.amazonaws.com/testmyapp:latest'
                    }
                }
            }
            stage('Push Image to ECR'){

                steps{

                    script{
                        
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 496157679619.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 496157679619.dkr.ecr.us-east-1.amazonaws.com/testmyapp:latest'

                    }
                }
                
            }
    //     stage('Connect with Cluster'){

    //     steps{

    //         script{

    //             sh """
    //             aws configure set aws_access_key_id "$ACCESS_KEY"
    //             aws configure set aws_secret_access_key "$SECRET_KEY"
    //             aws configure set region "${params.region}"


    //             aws eks --region ${params.region} update-kubeconfig --name ${params.cluster}
    //             """
    //         }
    //     }
    //   }
    //     stage('Deployment on eks cluster'){

    //     when{ expression { params.action == 'apply'} }

    //     steps{

    //         script{

    //            def apply = false

    //            try{
    //             input message: 'please confirm apply to initiate the deployment',  ok: 'Ready to apply the config ?'
    //             apply = true
    //            }catch(err){
    //             apply = false
    //             currentBuild.result ='UNSTABLE'
    //            }
    //            if(apply){
    //             sh """
    //                 kubectl apply -f .
    //             """
    //            }
    //         }
    //     }
    // }
 }      
}