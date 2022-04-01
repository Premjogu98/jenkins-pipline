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
                    echo "======  Build and Push Start  ======"
                    // dir('/home/diycam/RDX/') { // Build and push docker image
                    //     // try { 
                    //     //     sh 'docker buildx create --name armbuilder'
                    //     // }
                    //     // catch (Exception e) {
                    //     //     echo "Exception occurred: ${e.toString()} ${skipRemainingStages}"
                    //     // }
                        
                    //     sh 'docker run --rm --privileged multiarch/qemu-user-static --reset -p yes'
                        
                    //     for (image_name in [ 'service', 'frontend', 'base', 'user_info', 'socketserver','camera']) { // Build all this services
                    //         sh "docker buildx bake --builder armBuilder -f docker-compose.yml --push --set *.platform=linux/amd64,linux/arm64 ${image_name} --no-cache"
                    //         sleep 3 // Sleep 3sec
                    //         echo "*** Build Completed: ${image_name} ***"
                    //     }
                    // }
                    echo "======  Build and Push END  ======"
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
                        echo "======  Testing Start  ======"
                        echo "======  Testing End  ======"
                    }
                }
                stage('Testing on node_222') {
                    agent {
                        label "node_222"
                    }
                    steps{
                        script{
                            echo "======  Docker Image Pull Start  ======"

                            dir('/home/diycam/RDX/'){
                                container_id = sh(script: 'docker service ls -q',returnStdout: true).trim()
                                def containerid_list = container_id.split('\n')
                                echo "docker container list ${containerid_list}"
                                for (service_id in containerid_list){
                                    sh "docker service rm ${service_id}"
                                }
                                sh 'docker stack rm rdx'
                                sleep 3
                                sh 'docker-compose -f docker-compose-prod.yml pull'
                                sleep 10
                                sh 'docker stack deploy -c docker-compose-prod.yml rdx'
                                sleep 5
                                sh 'sudo service host start'
                            }
                            
                            echo "======   Docker Image Pull End  ======"
                        }
                    }
                }
            }
        }
    }
}
