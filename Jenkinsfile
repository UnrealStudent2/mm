pipeline {
    agent any

    stages {

        stage('Copy ROM') {
            steps {
                echo 'Setting up ROM...'
                sh 'cp /usr/local/etc/roms/baserom_mm.z64 baserom.z64'
            }
        }
        stage('Build') {
            when {
                not {
                    branch 'master'
                }
            }
            steps {
                sh 'make -j init'
            }
        }
        stage('Report Progress') {
            when {
                branch 'master'
            }
            steps {
                sh 'python3 progress.py csv >> /var/www/html/reports/progress_mm.csv'
                sh 'python3 progress.py csv -m >> /var/www/html/reports/progress_mm_matching.csv'
                sh 'python3 progress.py shield-json > /var/www/html/reports/progress_mm_shield.json'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}