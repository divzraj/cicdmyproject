pipeline{
    agent any
    /*tools{
        jdk "java"
        maven "mvn"
    }*/
    stages{
        stage("Fetch code from repo"){
            steps{
                git branch: main, url: "git@github.com:divzraj/cicdmyproject.git"
            }
        }
    }
}
