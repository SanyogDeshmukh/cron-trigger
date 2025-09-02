 pipeline {
 
     agent {
         label {
             label params.nodeLabel
         }
     }

    triggers {
        // Run s390x builds every Sunday at midnight
        //cron('H 0 * * 0')
        cron('H/5 * * * *')
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
            def causes = currentBuild.getBuildCauses()
            def triggeredByCron = causes.any { it._class == 'hudson.triggers.TimerTrigger$TimerTriggerCause' }

            if (params.nodeLabel == 's390x' && !triggeredByCron) {
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
                 sh 'chmod +x script.sh'
                 sh './script.sh'
             }
         }
     }
 }
