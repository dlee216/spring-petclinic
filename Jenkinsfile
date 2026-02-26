pipeline {
    agent any

    triggers {
        cron('H/5 * * * 4')
    }

    stages {
        stage('Build') {
            steps {
                bat './mvnw.cmd clean package -DskipTests'
            }
        }

        stage('Test & JaCoCo Coverage') {
            steps {
                bat './mvnw.cmd test jacoco:report'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    jacoco(
                        execPattern: '**/target/jacoco.exec',
                        classPattern: '**/target/classes',
                        sourcePattern: '**/src/main/java',
                        exclusionPattern: '**/src/test*'
                    )
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
