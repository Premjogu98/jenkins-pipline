pipeline {
    agent {label "master_node"}

    stages {
        stage('Pull Unreleased Branch Code') {
            steps {
                echo "Pull Start"
                echo "Pull End"
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
                            sh 'ifconfig etho'
                            echo "Push End"
                        }
                    }
                }
<<<<<<< HEAD
                stage('Testing on node_222') {
                    agent {
                        label "node_222"
                    }
                    steps{
                        script{
                            echo "Testing Start"
                            sh 'ifconfig etho'
                            echo "Testing End"
                        }
                    }
                }
=======
                // stage('Parallel node_140pc'){
                //     agent {
                //         label "node_140pc"
                //     }
                //     steps{
                //         script{
                //             echo "node_140pc"
                //             sh 'ifconfig'
                //         }
                //     }
                // }
>>>>>>> 1ddcfe10b8df1a9340e55cf693694c98b2ef49b0
            }
        }
    }
}