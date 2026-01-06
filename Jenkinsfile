pipeline {
    agent {
        node {
            label "Linux && java17"
        }
    }

    stages {

        stage("Build") {
            steps {
                echo("Start Build")
                sh("./mvnw clean compile test-compile")
                echo("Finish Build")
            }
        }

        stage("Test") {
            steps {
                echo("Start test")
                sh ("./mvnw test")
                echo("Finish test")
            }
        }

        stage("Deploy") {
            steps {
                echo "Hello Deploy 1"
                sleep(5)
                echo "Hello Deploy 2"
                echo "Hello Deploy 3"
            }
        }
    }



    post {
        always {
            echo "I will always say Hello again!"
        }
        success {
            echo "Yay, success"
        }
        failure {
            echo "Oh no, failure"
        }
        cleanup {
            echo "Don't care success or error"
        }
    }
}
