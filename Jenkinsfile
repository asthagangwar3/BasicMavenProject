pipeline {
    agent {label'My_Node'}

  tools {
      jdk 'jdk11'}
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/asthagangwar3/BasicMavenProject.git'

                // Run Maven on a Unix agent.
                bat "mvn clean install"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        
        stage('SonarQube') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat "D:/Softwares_Required/sonar-scanner-4.1.0.1829-windows/bin/sonar-scanner.bat"
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
                }
            }   
        }
    }
}
