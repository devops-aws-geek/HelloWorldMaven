pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps {
                withMaven(maven : 'apache-maven-3.9.6'){
                        sh "mvn clean compile"
                }
            }
        }
        stage('Test'){
            steps {
                withMaven(maven : 'apache-maven-3.9.6'){
                        sh "mvn test"
                }

            }
        }
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh '/home/vagrant/sw/maven/bin/mvn sonar:sonar -Dsonar.projectKey=DevOpsCICD -Dsonar.sources=./src/main/'
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    // Requires SonarScanner for Jenkins 2.7+
                    waitForQualityGate abortPipeline: true
                }
            }
			}
        stage('Deploy') {
            steps {
               withMaven(maven : 'apache-maven-3.9.6'){
                        sh "mvn deploy"
                }

            }
        }
    }
}
