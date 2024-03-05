pipeline{
    agent any
    /*tools{
        jdk "java"
        maven "mvn"
    }*/
    stages{
        stage("Fetch code from repo"){
            steps{
                git "git@github.com:divzraj/cicdmyproject.git"
            }
        }
    }
}