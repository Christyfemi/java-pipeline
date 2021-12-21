pipeline {
    agent any

    tools {
        // auto installed maven
        maven "Maven3.8.4"
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
        
        stage('Sonar stage')
        {
            environment{
            scannerScanner = tool 'Sonar'
            }
            steps{
                withSonarQubeEnv('SonarServer')
                {
                     sh "${scannerScanner}/bin/sonar-scanner -Dsonar.projectKey=java_maven -Dsonar.sources=. -Dsonar.java.binaries=target/classes/com/mycompany/app/ "
                }
            }
            
        }
        
        stage('Quality gate')
        {
            steps{
                waitForQualityGate abortPipeline: true
            }
        }
        
        stage('Delpoy')
        {
            steps{
                s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'automationframeworkcommonsteps', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'eu-west-2', showDirectlyInBrowser: false, sourceFile: 'target/*.jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'S3', userMetadata: []            }
        }
    }
}


