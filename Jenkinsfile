pipeline {
    agent any
    stages 
    {

        stage('Init'){
            steps {
                    script {
                        lastCommitInfo = sh(script: "git log -1", returnStdout: true).trim()
                        commitContainsSkip = sh(script: "git log -1 | grep 'skip ci'", returnStatus: true)
                        //if commit message contains skip ci
                        if(commitContainsSkip == 0) {
                            skippingText = " Skipping Build for *${env.BRANCH_NAME}* branch."
                            currentBuild.result = 'ABORTED'
                            error('BUILD SKIPPED')
                        }
                    }
            }
        } 

        // Uploading Docker images into Docker hub
        stage('Pushing to Docker Hub') {              
        steps{
            script {
                
                    sh " git clone https://github.com/Manish7992/hello-test-rancher.git"
                    sh " cd hello-test-rancher/hello-rancher"
                    sh "docker build -t manish8757/rancher:${GIT_COMMIT} hello-test-rancher/hello-rancher"
                    sh "docker login -u manish8757 -p manish123@"
                    sh "docker push manish8757/rancher:${GIT_COMMIT} "
            }
            }
        }       

    }
    post{
        always {
                deleteDir() 
            }

    }     
} //pipeline
