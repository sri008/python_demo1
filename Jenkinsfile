pipeline {
   agent any

   stages {
      stage('SCM Checkout') {
         steps {
            git 'https://github.com/sri008/python_demo1.git'
         }
      }
      stage('Build') {
         steps {
            sh label: '', script: '''python3 --version
            python3 -m venv env
            source env/bin/activate
            pip3 install --user -r requirements.txt'''
         }
      }
      stage('Install') {
         steps {
            sh 'pip3 install --user nose coverage nosexcover pylint'
         }
      }
      stage('Analysis') {
         steps {
            withSonarQubeEnv('sonar'){
                sh '/opt/sonar-scanner-3.3.0.1492-linux/bin/sonar-scanner -Dsonar.projectName=pythonproject -Dsonar.projectVersion=2 -Dsonar.projectKey=pythonproject -Dsonar.python.coverage.reportPath=coverage.xml -Dsonar.analysis.mode= -Dsonar.sources=.'
            }
         }
      }
      stage('Test') {
         steps {
            sh 'python3 test.py'
            sh 'deactivate'
         }
         post{
            always{
                junit 'test-reports/*.xml'
            }
            cleanup{
                echo 'Clean up the workspace'
                deleteDir() /*clean up our workspace*/
            }
         }
      }
   }
}
