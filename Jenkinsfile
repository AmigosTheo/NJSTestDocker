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
      }

      stage("Final check - Workspace and ls..."){
        echo "${WORKSPACE}"
        sh 'ls'
      }

      } catch(Exception e) {
        // Do something with the exception
          println "${e}"
          error "Program failed, please read logs..."
        // Do I need to add an exception to remove failed Docker Containers?
        }
}
