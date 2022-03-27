pipeline{
    
    tools{
        jdk 'Java_Home'
        maven 'myMaven'
    }
    agent {label 'Slave_nodes'}
    stages{
        stage('Clone a repo')
        {
            steps{
                git 'https://github.com/timjar3/DevOpsClassCodes.git'
            }
        }
        stage('Compile'){
            
            steps{
                sh 'mvn compile'
            }
        }
        
        stage('CodeReview')
        {
        steps{
            sh 'mvn pmd:pmd'
        }
    }
    stage('Unit Test'){
        steps{
            sh 'mvn test'
        }
    }
       
    stage('CodeCoverage'){
        steps{
            sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
        }
    }
    
    stage('package'){
        steps{
            sh 'mvn package'
        }
    }
    
    stage('docker build'){
        steps{
            sh "docker build -t timjar3/addbook:$BUILD_NUMBER ."
        }
    }
    
    stage('docker build'){
        steps{
            withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerhubPwd')]) {
            sh "docker login -u timjar3 -p ${dockerhubPwd}"
            sh "docker push -t timjar3/addbook:$BUILD_NUMBER ."
        }
    }
    
    }
    
}
