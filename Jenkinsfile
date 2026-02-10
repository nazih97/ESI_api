pipeline {
    agent any
// mot de passe : bino ibey wugx vtnv

// token classic : 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
    stages {
        stage('test') {
            steps {
                bat 'mvn test'
                junit 'target/surefire-reports/*.xml'

                cucumber reportTitle: 'API Report',
                   fileIncludePattern: 'target/example-report.json'
                   endsLimit: 10


                   recordCoverage(tools: [[parser: 'JACOCO']],
                           id: 'jacoco', name: 'JaCoCo Coverage',
                           sourceCodeRetention: 'EVERY_BUILD',
                           qualityGates: [
                                   [threshold: 60.0, metric: 'LINE', baseline: 'PROJECT', unstable: true],
                                   [threshold: 60.0, metric: 'BRANCH', baseline: 'PROJECT', unstable: true]])
            }
        }



     }
}
