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
        docker build -t nodejstest .
        docker images
        ls -a
        docker rmi nodejstest
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
        currentBranch = scm.branches[0].name
        jiraSendDeploymentInfo branch: "${currentBranch}", site: 'techamigos.atlassian.net', environmentId: 'eu-west-2', environmentName: 'eu-west-2', environmentType: 'development'

      }

      } catch(Exception e) {
        // Do something with the exception
          println "${e}"
          error "Program failed, please read logs..."
        // Do I need to add an exception to remove failed Docker Containers?
        }
}
