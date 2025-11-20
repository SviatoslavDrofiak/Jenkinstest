pipeline {
    agent any

    stages {
        stage('Install Apache') {
            steps {
                echo 'Installing Apache2...'
                sh '''
                    apt-get update
                    apt-get install -y apache2
                    systemctl enable apache2
                    systemctl start apache2
                '''
            }
        }

        stage('Check Apache Logs') {
            steps {
                echo 'Checking Apache logs for 4xx and 5xx errors...'
                sh '''
                    LOG_FILE="/var/log/apache2/access.log"
                    if [ -f "$LOG_FILE" ]; then
                        echo "Checking for 4xx errors..."
                        grep ' 4[0-9][0-9] ' $LOG_FILE || echo "No 4xx errors"
                        echo "Checking for 5xx errors..."
                        grep ' 5[0-9][0-9] ' $LOG_FILE || echo "No 5xx errors"
                    else
                        echo "Log file not found!"
                    fi
                '''
            }
        }
    }
}
