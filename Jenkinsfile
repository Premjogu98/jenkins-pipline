import java.text.SimpleDateFormat

def skipRemainingSteps = true
def deployment_process_done = false

def str_front_commit_date = null
def str_back_commit_date = null

def date_front_commit_date = null
def date_front_last_commit_date = null

def date_back_commit_date = null
def date_back_last_commit_date = null

def date_format = "yyyy-MM-dd-HH:mm"

pipeline {
    //Setting the environment variables DISABLE_AUTH and DB_ENGINE
    environment {
        GIT_ACCESS_TOKEN = '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'
        GIT_USERNAME = 'Premjogu98'
    }
    agent none
    stages {
        stage('stage1') {
            failFast true // You can force your parallel stages to all be aborted when one of them fails, by adding failFast true to the stage containing the parallel.
            parallel {
                
                stage('MasterNode Git pull process and docker build push process') {
                    agent {
                        label "master_node"
                    }
                    steps{
                        script {
                            if (skipRemainingSteps == true){
                                try {

                                    echo 'Pulling Frontend'

                                    dir('/home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend') { // Pull Frontend Code from github 
                                        git branch: '1.0.1_dev', url: "https://${GIT_USERNAME}:${GIT_ACCESS_TOKEN}@github.com/indiadiycam/rdx_frontend.git"
                                    }
                                    
                                    dir('/home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend'){ // Path to frontend folder

                                        sh 'pwd'
                                        sh 'dir'
                                        
                                        str_front_commit_date = sh(script: 'git log -1 --format="%at" | xargs -I{} date -d @{} +%Y-%m-%d-%H:%M',returnStdout: true).trim() // get last push date from git
                                        def str_front_last_commit_date = readFile(file: 'last_commit.txt') // get last push date from txtfile

                                        echo "str_front_commit_date : ${str_front_commit_date}"
                                        echo "str_front_last_commit_date : ${str_front_last_commit_date}"
                                        
                    
                                        date_front_commit_date = new SimpleDateFormat(date_format).parse(str_front_commit_date) // String to Datetime formate
                                        date_front_last_commit_date = new SimpleDateFormat(date_format).parse(str_front_last_commit_date) // String to Datetime formate
                                        
                                        echo "date_front_commit_date : ${date_front_commit_date}"
                                        echo "date_front_last_commit_date : ${date_front_last_commit_date}"
                                    }
                                } catch (Exception e) {
                                    echo "Exception occurred: ${e.toString()} ${skipRemainingStages}"
                                    skipRemainingSteps = false
                                }
                            }else{
                                echo 'Some Errors Found So Skipped This Part'
                            }
                        }
                        script {
                            if (skipRemainingSteps == true){
                                try {
                                    echo 'Pulling Backend'

                                    dir('/home/diycam/RDX/') { // Pull Backend Code from github 
                                        git branch: 'dipesh_dev', url: 'https://${GIT_USERNAME}:${GIT_ACCESS_TOKEN}@github.com/dipesh-adekar/rdx.git'
                                    }
                                    
                                    dir('/home/diycam/RDX/'){ // Path to backend folder
                                        sh 'pwd'
                                        sh 'dir'
                                        skipRemainingStages = true

                                        str_back_commit_date = sh(script: 'git log -1 --format="%at" | xargs -I{} date -d @{} +%Y-%m-%d-%H:%M',returnStdout: true).trim() // get last push date from git
                                        def str_back_last_commit_date = readFile(file: 'last_commit.txt') // get last push date from txtfile

                                        echo "str_back_commit_date : ${str_back_commit_date}"
                                        echo "str_back_last_commit_date : ${str_back_last_commit_date}"
                                        
                                        date_back_commit_date = new SimpleDateFormat(date_format).parse(str_back_commit_date) // String to Datetime formate
                                        date_back_last_commit_date = new SimpleDateFormat(date_format).parse(str_back_last_commit_date) // String to Datetime formate
                                        
                                        echo "date_back_commit_date : ${date_back_commit_date}"
                                        echo "date_back_last_commit_date : ${date_back_last_commit_date}"
                                    
                                    }
                                } catch (Exception e) {
                                    echo "Exception occurred: ${e.toString()} ${skipRemainingStages}"
                                    skipRemainingSteps = false
                                }
                            }else{
                                echo 'Some Errors Found So Skipped This Part'
                            }
                        }
                        script{
                            if (skipRemainingSteps == true){
                                try{
                            
                                    if (date_front_commit_date > date_front_last_commit_date || date_back_commit_date > date_back_last_commit_date){

                                        dir('/home/diycam/RDX/') {
                                            for (build_name in [ 'service', 'frontend', 'base', 'user_info', 'socketserver','camera']) { // Build all this services
                                                // sh "docker buildx bake --builder armBuilder -f docker-compose.yml --push --set *.platform=linux/amd64,linux/arm64 ${build_name} --no-cache"
                                                sleep 3 // Sleep 3sec
                                                echo "Build Completed: ${build_name}"
                                            }
                                            writeFile(file: 'last_push.txt', text: str_front_commit_date) // overwrite latest commited date to previous date
                                        }
                                        dir('/home/diycam/Desktop/jenkins_master_files/workspace/pipeline_projects/pull_git_code_to_slaves/frontend'){
                                            writeFile(file: 'last_push.txt', text: str_back_commit_date) // overwrite latest commited date to previous date
                                        }
                                        deployment_process_done = true
                                        echo 'NOW YOU CAN DEPLOY CODE TO CLOUD AND TEST IT'
                                    }else{
                                        echo 'Nothing Changed'
                                    }
                                }catch (Exception e) {
                                    echo "Exception occurred: ${e.toString()} ${skipRemainingStages}"
                                    skipRemainingSteps = false
                                }
                                
                            }else{
                                echo 'Some Errors Found So Skipped This Part'
                            }
                        }
                    }
                }
            }
        
        }
    }
}
