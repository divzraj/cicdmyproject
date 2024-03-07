pipeline{
    agent any
    tools{
        jdk "java8"
        maven "mvn"
    }
    environment{
        scannerHome = tool 'sonarscanner'
    }

    stages{
        stage("Fetch code from repo"){
            steps{
                git branch: "main", url: "git@github.com:divzraj/cicdmyproject.git"
            }
        }

        stage(" Build the code"){
            steps{
                sh "mvn clean install -DskipTests"
            }
            post{
                success{
                    echo "Now archieving"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('unit test'){
            steps{
                sh "mvn test"
            }
        }

        stage("Integration check"){
            steps{
                sh "mvn verify -DskipTests"
            }
        }
        

        stage("Code analysis with checkstyle"){
            steps{
                sh "mvn checkstyle:checkstyle"
            }
            post{
                success{
                    echo "Analysis report generated"
                }
         
                }
        }

        stage("code analysis with sonarqube"){
            steps{
                withSonarQubeEnv('sonarserver'){
                 sh '''${scannerHome}/bin/sonar-scanner  -Dsonar.projectKey=cicdproject \
                   -Dsonar.projectName=cicdproject \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''

                }
                timeout(time: 10, unit: 'MINUTES') {
                 waitForQualityGate abortPipeline: true
                 }
        }

}
}
}