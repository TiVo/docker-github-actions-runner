#!groovy

imageName = 'gh-action-runner-base'
imageVersion = '2.299.0' // match gh action runner version
imageTargetPlatform = 'linux-x64' // build linux-x64 only

def buildId 


pipeline {
    agent { label 'docker' }

    options {
        timeout( time: 1, unit: 'HOURS' )
        buildDiscarder( logRotator(daysToKeepStr: '15', numToKeepStr: '10' ) )
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME ==~ /(tivo-build)/) {
                        buildId = "${imageVersion}-${imageTargetPlatform}"
                        docker.withRegistry( 'https://docker.tivo.com', 'docker-registry' ) {
                            image = docker.build( "devops/${imageName}:${buildId}", "-f Dockerfile.tivo-build --build-arg TARGETPLATFORM=${imageTargetPlatform} ." )
                            image.push()
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Yay SUCCESS ..."
        }

        failure {
            echo "Boo FAILURE ..."
        }
    }
}
