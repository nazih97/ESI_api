pipeline {
    agent any

    stages {

        stage('Test') {
            steps {
                bat 'mvn test'
                junit 'target/surefire-reports/*.xml'

                cucumber reportTitle: 'API Report',
                         fileIncludePattern: 'target/example-report.json'

                recordCoverage(
                    tools: [[parser: 'JACOCO']],
                    id: 'jacoco',
                    name: 'JaCoCo Coverage',
                    sourceCodeRetention: 'EVERY_BUILD',
                    qualityGates: [
                        [threshold: 80.0, metric: 'LINE', baseline: 'PROJECT', unstable: true],
                        [threshold: 80.0, metric: 'BRANCH', baseline: 'PROJECT', unstable: true]
                    ]
                )
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

                powershell """
                \$body = @{
                    text = "Deploy completed successfully!"
                } | ConvertTo-Json

                Invoke-RestMethod -Uri "${SLACK_notif}" `
                                  -Method Post `
                                  -ContentType "application/json" `
                                  -Body \$body
                """
            }
        }


        stage('realise'){
        steps{
bat """
curl -X POST https://api.github.com/repos/nazih97/ESI_api/releases ^
-H "Authorization: Bearer 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" ^
-H "Accept: application/vnd.github+json" ^
-H "Content-Type: application/json" ^
-d "{\\"tag_name\\":\\"v%VERSION%\\",\\"name\\":\\"Release v%VERSION%\\",\\"body\\":\\"Production release\\",\\"draft\\":false,\\"prerelease\\":false}"
"""

        }}


    }
}
