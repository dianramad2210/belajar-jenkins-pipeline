pipeline {
    agent none

    environment {
        AUTHOR = "Dian Ramadhani"
        EMAIL = "105841116323@student.unismuh.ac.id"
        WEB = "https://www.unismuh.ac.id"
    }
    
    parameters {
        string(name: "NAME", defaultValue: "Guest", description: "What  is your name?")
        text(name: "DESCRIPTION", defaultValue: "Guest", description: "Tell me about you")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to Deploy?")
        choice(name: "SOCIAL_MEDIA", choices: ['Instagram', 'Facebook', 'Twitter'], description: "Which Social Media")
        password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
        
    }

    // triggers {
    //     cron("*/5 * * * *")
    // }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }

    stages {
        stage("OS Setup") {
            matrix {
                axes {
                    axis {
                        name "OS"
                        values "Linux", "Winndows", "Mac"
                    }
                    axis {
                        name "ARC"
                        values "32", "64"
                    }
                }
                 stages {
                stage("OS Setup") {
                    agent {
                        node {
                            label "Linux && Java17"
                        }
                    }
                    steps {
                        echo("Setup ${OS} ${ARC}")
                    }
                }
            }
            }
        }

        stage("Preparation") {
            parallel {
                stage("Prepare Java") {
                     agent {
                        node {
                        label "Linux && java17"
                        }
                    }
                    steps {
                        echo("Prepare Java")
                        sleep(5)
                    }
                }
                  stage("Prepare Maven") {
                     agent {
                        node {
                        label "Linux && java17"
                        }
                    }
                    steps {
                        echo("Prepare Maven")
                        sleep(5)
                    }
                }
            }
        }

        stage("Parameter") {
            agent {
               node {
                label "Linux && java17"
                }
            }
        steps {
            echo "Hello ${params.NAME}"
            echo "You description ${params.DESCRIPTION}"
            echo "Your social media is ${params.SOCIAL_MEDIA}"
            echo "Need to deploy : ${params.DEPLOY} to deploy!"
            echo "Your secret is ${params.SECRET}"
            }
        }

           stage("Prepare") {
                environment {
                    APP = credentials("dian_rahasia")
                }

            agent {
                node {
                    label "Linux && java17"
               }
            }
            steps {
                echo("Author ${AUTHOR}")
                echo("Email ${EMAIL}")
                echo("Web ${WEB}")
                echo("Start Job : ${env.JOB_NAME}")
                echo("Start Build : ${env.BUILD_NUMBER}")
                echo("Branch Name : ${env.BRANCH_NAME}")
                echo("App User : ${APP_USR}")
                sh('echo "App Password : $APP_PSW" > "rahasia.txt"')

            }
        }

        stage("Build") {

            agent {
                node {
                    label "Linux && java17"
               }
            }
            steps {

                script {
                    for (int i = 0; i < 10; i++) {
                        echo("Script ${i}")
                    }

                }

                echo("Start Build")
                sh("./mvnw clean compile test-compile")
                echo("Finish Build")
            }
        }

        stage("Test") {
            agent {
                node {
                    label "Linux && java17"
                }
            }
            steps {

                script {
                    def data = [
                        "firstName" : "Dian",
                        "lastName" : "Ramadhani"
                    ]
                    writeJSON(file: "data.json", json: data)
                }

                echo("Start test")
                sh ("./mvnw test")
                echo("Finish test")
            }
        }

        stage("Deploy") {
            input {
                message "Can we deploy?"
                ok "Yes, of course"
                submitter "dianramad_2210, james"
                parameters {
                    choice(name: "TARGET_ENV", choices: ['DEV', 'QA', 'PROD'], description: "Which environment?")
                }
            }
            agent {
                node {
                    label "Linux && java17"
                }
            }
            steps {
                echo("Deploy to ${TARGET_ENV}")
            }
        }

        stage("Release") {
            when {
                expression {
                    return params.DEPLOY
                }
            }
            agent {
                node {
                    label "Linux && java17"
                }
            }
            steps {
                echo("Release it")
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
