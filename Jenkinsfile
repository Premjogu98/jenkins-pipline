pipeline {
    agent {label "master_node"}

    stages {
        stage('Pull Unreleased Branch Code') {
            steps {
                script{
                    echo "======  Pulling Start  ======"
                    dir('/home/diycam/RDX/') { // Pull code from github 
                        git branch: 'unreleased', url: "https://${env.GIT_USERNAME}:${env.GIT_ACCESSTOKEN}@github.com/dipesh-adekar/rdx.git"
                        }
                    echo "======  Pulling End  ======"
                }
            }
        }
        stage('Build and Push Docker Images') {
            steps {
                echo "Build and Push Start"
                echo "Build and Push End"
            }
        }
        stage('Testing on Jetson node_222'){
            failFast true // You can force your parallel stages to all be aborted when one of them fails, by adding failFast true to the stage containing the parallel.
            parallel {
                stage('Pull Docker Images to Node 222') {
                    agent {
                        label "node_222"
                    }
                    steps{
                        script{
                            echo "Push Start"
                            sh 'ifconfig eth0'
                            echo "Push End"
                        }
                    }
                }
                stage('Testing on node_222') {
                    agent {
                        label "node_222"
                    }
                    steps{
                        script{
                            echo "Testing Start"
                            sh 'ifconfig eth0'
                            echo "Testing End"
                        }
                    }
                }
            }
        }
    }
}
