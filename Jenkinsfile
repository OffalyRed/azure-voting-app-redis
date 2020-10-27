pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
         stage('Docker Test') {
         steps {
            pwsh(script: 'Write-Output "docker images -a"')
            }
        }
        stage('Docker Test') {
         steps {
            pwsh(script: 'docker images -a')
            }
        }
        stage('Docker Build') {
         steps {
            pwsh(script: """
               cd azure-vote/
               docker build -t offaly/jenkins-pipeline:v1.0 .
               docker images -a
               cd ..
            """)
            pwsh(script: 'docker images -a')
            }
        }
        stage('DB') {
            steps {
                echo 'Finished Docker Build'
            }
        }
        stage('Start test app') {
         steps {
            pwsh(script: """
               docker-compose up -d
               ./scripts/test_container.ps1
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            pwsh(script: """
               pytest ./tests/test_sample.py
            """)
         }
      }
      stage('Stop test app') {
         steps {
            pwsh(script: """
               docker-compose down
            """)
         }
      }
      stage('Goodbye') {
            steps {
                echo 'Goodbye World'
            }
        }
    }
}
