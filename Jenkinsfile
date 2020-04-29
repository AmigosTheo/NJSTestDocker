#!groovy
import groovy.json.*

node('master') {

    echo "${WORKSPACE}"

    try {

      stage("1: Develop Init"){
      cleanWs()
      deleteDir()
      checkout scm

      }

      stage("2: Creating docker container..."){
        sh'''
        ls -a
        '''

        // This is the Jira Feedback Code for the Build Info stage
        println scm.branches[0].name
        currentBranch = scm.branches[0].name
        jiraSendBuildInfo branch: "${currentBranch}", site: 'techamigos.atlassian.net'
      }

      stage("Final check - Workspace and ls..."){
        echo "${WORKSPACE}"
        sh 'ls'
        jiraSendDeploymentInfo branch: "${currentBranch}", site: 'techamigos.atlassian.net', environmentId: 'eu-west-2', environmentName: 'eu-west-2', environmentType: 'development'
        jiraSendDeploymentInfo branch: "${currentBranch}", site: 'techamigos.atlassian.net', environmentId: 'eu-west-2', environmentName: 'eu-west-2', environmentType: 'staging'

      }

      stage("BUILD SUCCEED") {
        slackSend (color: '#00FF00', message: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' requires further authorisation")
      }

      } catch(Exception e) {
        // Do something with the exception
          println "${e}"
          error "Program failed, please read logs..."
        // Do I need to add an exception to remove failed Docker Containers?
        }
}
