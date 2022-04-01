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
                script{
                    echo "Build and Push Start"
                    dir('/home/diycam/RDX/') { // Build and push docker image
                        sh 'docker buildx create --name armbuilder'
                        sh 'docker run --rm --privileged multiarch/qemu-user-static --reset -p yes'
                        for (image_name in [ 'service', 'frontend', 'base', 'user_info', 'socketserver','camera']) { // Build all this services
                            sh "docker buildx bake --builder armBuilder -f docker-compose.yml --push --set *.platform=linux/amd64,linux/arm64 ${image_name} --no-cache"
                            sleep 3 // Sleep 3sec
                            echo "Build Completed: ${image_name}"
                        }
                    }
                    echo "Build and Push End"
                }
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
