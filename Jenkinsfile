pipeline {
    agent any

    stages {

        stage('Always Run') {
            steps {
                echo "This runs for every branch"
            }
        }

        stage('Run Only on dev') {
            when {
                branch 'dev'
            }
            steps {
                echo "This is dev branch"
            }
        }

        stage('Run Only on main') {
            when {
                branch 'prod'
            }
            steps {
                echo "This is main branch"
            }
        }
    }
}
