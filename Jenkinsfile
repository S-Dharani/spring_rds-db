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
                echo "Stopping existing application..."
                pkill -f "student_details-0.0.1-SNAPSHOT.jar" || true

                sleep 5

                echo "Starting application..."
                nohup java -jar target/student_details-0.0.1-SNAPSHOT.jar > app.log 2>&1 &

                sleep 15

                echo "Checking application status..."

                if ps -ef | grep "student_details-0.0.1-SNAPSHOT.jar" | grep -v grep > /dev/null
                then
                    echo "Application Started Successfully"
                else
                    echo "Application Failed to Start"
                    echo "========== APP LOG =========="
                    cat app.log
                    exit 1
                fi
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
