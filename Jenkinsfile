pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/wiztex/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t wiztex/devops-integration .'
                }
            }
        }
       stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([usernamePassword(credentialsId: 'dockerhubpwd', usernameVariable: 'username', passwordVariable: 'password')]) {
                   sh 'docker login -u ${username} -p ${password}'

}
                   sh 'docker push wiztex/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'minikubcfg')
                }
            }
        }
    }
}
