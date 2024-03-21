pipeline{
    agent any
    tools{
        jdk "openjdk8"
        maven "mvn"
    }
    environment{
        scannerHome = tool 'sonarscanner'
        imageregistry_username_imagename = "divyar123nag/cicdproject"
        credfordockerlogin = "dockerhublogin"
    }

    stages{
        stage("Fetch code from repo"){
            steps{
                git branch: "main", url: "git@github.com:divzraj/cicdmyproject.git"
            }
        }

        stage("Build the code"){
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


        stage("Build Image with Docker"){
            steps{
                script{
                    docImage = docker.build imageregistry_username_imagename + ":V$BUILD_NUMBER"
                }
            }
        }

        stage("Push Image to Dockerhub"){
            steps{
                script{
                    docker.withRegistry('', credfordockerlogin ){
                    docImage.push("V$BUILD_NUMBER")
                    docImage.push('latest')
                    }
                }
            }
        }

        stage("Remove unused Docker Images"){
            steps{
                sh " docker rmi $imageregistry_username_imagename:V$BUILD_NUMBER"
            }
        }
}
}