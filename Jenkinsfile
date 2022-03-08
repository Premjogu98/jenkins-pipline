
def skipRemainingStages = false

pipeline {
    agent none
    stages {
        stage('stage1') {
            failFast true
            parallel {
                stage('Master Node Clone Git Repo') {
                    agent {
                        label "master_node"
                    }
                    
                    steps{
                        script {
                            script {
                                try {
                                    sh 'cd rdx'
                                } catch (Exception e) {
                                    echo "Exception occurred: ${e.toString(), skipRemainingStages}"
                                    sh 'pwd'
                                }
                                if (skipRemainingStages == true) {
                                    echo 'I only execute on the master branch'
                                } else {
                                    echo 'I execute elsewhere'
                                }
                            }
                        }
                    }
                    
                    
                    // steps{
                    //     echo 'Pulling Frontend'
                    //     dir('/home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend') {
                    //         // git 'https://Premjogu98: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@github.com/indiadiycam/rdx_frontend.git'   
                    //         git  branch: '1.0.1_dev', credentialsId: '40be9959-83d3-4add-b9af-8b6b637d4e07', url: 'https://Premjogu98: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@github.com/indiadiycam/rdx_frontend.git'
                            
                    //     }
                    //     echo 'Pulling Frontend Completed'
                        
                    //     echo 'Pulling Backend'
                    //     dir ('/home/diycam/RDX/') {
                    //         git branch: 'dipesh', url: 'https://Premjogu98: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@github.com/dipesh-adekar/rdx.git'
                    //     }
                    //     echo 'Pulling Backend Completed'
                    
                    //     echo 'Installing Dependancies Frontend'
                    //     dir('/home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend') {
                    //         // git  branch: '1.0.1_dev', credentialsId: '40be9959-83d3-4add-b9af-8b6b637d4e07', url: 'https://Premjogu98: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@github.com/indiadiycam/rdx_frontend.git'
                    //         sh "pwd"
                    //         sh "sudo apt update"
                    //         sh 'npm install'
                    //     }
                    //     dir('/home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend/src') {
                    //         sh 'npm run build'
                    //         sh 'cp -r /home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend/build/* /home/diycam/RDX/frontend/html'
                    //     }
                    
                    //     echo "ALL Done"
                    // }
                        
                    
                }
            }

        }
    }
}
