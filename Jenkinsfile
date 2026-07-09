pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
            post {
                success {
                    echo 'Build Success'
                }
                failure {
                    echo 'Build Failed'
                }
            }
        }

        stage('Run Application') {
            steps {
                 sh '''
        pkill -f student_details || true

        java -jar target/student_details-0.0.1-SNAPSHOT.jar

        disown || true

        sleep 15

        ps -ef | grep student_details || true
        '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline Success'
        }
        failure {
            echo 'Pipeline Failed'
        }
        always {
            echo 'Pipeline Execution Completed'
        }
    }
}
