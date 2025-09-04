
 pipeline {
    agent none

    parameters {
        choice(
            name: 'nodeLabel',
            choices: ['amd64', 's390x'],
            description: 'Choose target architecture'
        )
    }

    triggers {
        // Run s390x builds every Sunday at midnight (for testing: every 5 min)
        cron('H/5 * * * *')
    }

    stages {
        stage('Decide Node') {
            steps {
                script {
                    // Detect if triggered by cron
                    def causes = currentBuild.getBuildCauses()
                    def triggeredByCron = causes.any { it._class == 'hudson.triggers.TimerTrigger$TimerTriggerCause' }

                    if (triggeredByCron) {
                        // Cron → force s390x
                        targetNode = 's390x'
                        echo "Triggered by CRON → running s390x"
                    } else {
                        // Manual or other triggers → use parameter (amd64 preferred)
                        targetNode = params.nodeLabel
                        echo "Manual/Other trigger → running ${targetNode}"
                    }
                }
            }
        }

        stage('Initialization') {
            agent { label "${targetNode}" }
            steps {
                echo "running on ${env.NODE_NAME}"
                echo "requested label: ${params.nodeLabel}"
                sh 'chmod +x script.sh'
                sh './script.sh'
            }
        }

        stage('Build') {
                echo "Building on ${env.NODE_NAME}"
        }

        stage('Test') {
                echo "Testing on ${env.NODE_NAME}"
        }

        stage('Deploy') {
                echo "Deploying on ${env.NODE_NAME}"
       }
    }
}
