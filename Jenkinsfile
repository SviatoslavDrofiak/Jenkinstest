pipeline {
    agent any

    environment {
        DEBIAN_FRONTEND = 'noninteractive'
    }

    stages {
        stage('Install Apache') {
            steps {
                echo 'Installing Apache2...'
                sh '''
                    sudo apt-get update
                    sudo apt-get install -y apache2
                    sudo systemctl enable apache2
                    sudo systemctl start apache2
                    sudo systemctl status apache2 || echo "Apache2 failed to start!"
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
                        echo "Apache log file not found. Is Apache running and serving requests?"
                    fi
                '''
            }
        }
    }
}
