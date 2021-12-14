pipeline {
    agent any

    tools {
        // auto installed maven
        maven "Maven"
    }

    stages {
        stage('cleanup')
        {
            steps{
                deleteDir()
            }
        }
        
        stage('Git Checkout') {
            steps {                
                git credentialsId: 'my-github-credentials', url: 'https://github.com/Christyfemi/java-pipeline.git'
            }

        }
        
        stage('Build') {
            steps{
                sh 'mvn clean install'
            }
        }
    }
}


