pipeline {
    agent any
// mot de passe : bino ibey wugx vtnv

// token classic : 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
    stages {
        stage('test') {
            steps {
                bat 'mvnw test'
                junit 'target/surefire-reports/*.xml'
            }
        }


     }
}
