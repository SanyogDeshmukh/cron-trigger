 pipeline {
 
     agent {
         label {
             label params.nodeLabel
         }
     }

    triggers {
        // Run s390x builds every Sunday at midnight
        cron('H 0 * * 0')
    }

    parameters {
        choice(
            name: 'nodeLabel',
            choices: ['amd64', 's390x'],
            description: 'Choose target architecture'
        )
    }
  
     stages {
        stage('Guard') {
            steps {
                script {
                    // Allow s390x builds only when triggered by the Sunday cron
                    if (params.nodeLabel == 's390x' &&
                        currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger.TimerTriggerCause) == null) {
                        echo "Skipping s390x build (not Sunday cron trigger)"
                        currentBuild.result = 'NOT_BUILT'
                        error("Aborting pipeline for non-scheduled s390x run")
                    }
                }
            }
        }

         stage('Initialization') {
             steps {
                 echo "running on ${env.NODE_NAME}"
                 sudo sh './script.sh'
             }
         }
     }
 }
