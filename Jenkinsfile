pipeline{
    agent any
    tools{
        jdk "java8"
        maven "mvn"
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
}
}
