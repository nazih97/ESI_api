pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                bat 'mvn test'
                junit 'target/surefire-reports/*.xml'

                cucumber reportTitle: 'API Report',
                   fileIncludePattern: 'target/example-report.json'
                   // endsLimit: 10


                   recordCoverage(tools: [[parser: 'JACOCO']],
                           id: 'jacoco', name: 'JaCoCo Coverage',
                           sourceCodeRetention: 'EVERY_BUILD',
                           qualityGates: [
                                   [threshold: 80.0, metric: 'LINE', baseline: 'PROJECT', unstable: true],
                                   [threshold: 80.0, metric: 'BRANCH', baseline: 'PROJECT', unstable: true]])
            }
        }

                 stage('Documentation') {
                      steps {
                          bat 'mvn.cmd javadoc:javadoc'
                      }
                      post {
                       always {
                                     publishHTML(target: [
                                         allowMissing: false,
                                         alwaysLinkToLastBuild: true,
                                         keepAll: true,
                                         reportDir: 'target/site/',
                                         reportFiles: 'index.html',
                                         reportName: 'Documentation'
                                     ])
                       }
                      }
                 }


                         stage('Build') {
                             steps {
                                 bat 'mvn.cmd package'
                                 archiveArtifacts artifacts: 'target/*.jar'
                             }
                         }

                      stage('Deploy') {
                          steps {
                              bat 'mvn.cmd deploy'
                          }
                      }


                         stage('Slack') {
                             steps {
                                 powershell """
                                 \$body =@{ text = "jenkins succes" } | ConvertTo-Json
                                 Invoke-RestMethod
                                   -Uri "${SLACK_notif}" `
                                   -Method Post `
                                   -ContentType "application/json" `
                                   -Body \$body
                                 """
                             }
                         }


     }
}
