#!/usr/bin/env groovy
// Liquibase declarative pipeline
//
//
pipeline {
agent any
  environment {
    PROJ="ora_release_example"
    GITURL="https://github.com/szandany"
    ENVIRONMENT_STEP="${params.step}"
    BRANCH="${params.pipeline}"
    LB_COMMAND="${params.command}"
    PATH="/root/liquibase:$PATH"
  }
  stages {

    stage ('Checkout') {
      steps {
        // checkout Liquibase project from repo
        sh '''
          { set +x; } 2>/dev/null
          cd /root/workspace
          if [ -d "$PROJ" ]; then rm -Rf $PROJ; fi
          git clone ${GITURL}/${PROJ}.git
          cd ${PROJ}
          git checkout $BRANCH
          git status
          '''
      } // steps for checkout stages
    } // stage 'checkout'

   stage ('liquibase commands'){
      steps {
        sh '''
          { set +x; } 2>/dev/null
          cd /root/workspace/${PROJ}
          liquibase --version
          echo "------------------------------------"
          echo "----------liquibase status----------"
          echo "------------------------------------"
          liquibase --classpath=/root/Drivers/ojdbc8.jar --url=${ORACLE_URL} --username=${ENVIRONMENT_STEP} --password=${PASSWORD} --changeLogFile=master.xml --contexts="${contexts}" --labels="${labels}" status --verbose
          echo "------------------------------------"
          echo "----------liquibase ${LB_COMMAND}----------"
          echo "------------------------------------"
          liquibase --classpath=/root/Drivers/ojdbc8.jar --url=${ORACLE_URL} --username=${ENVIRONMENT_STEP} --password=${PASSWORD} --changeLogFile=master.xml --contexts=${contexts} --labels=${labels} ${LB_COMMAND}
        '''
      } // steps
    }   // Environment stage

        stage ('Deleting project workspace'){
           steps {
             sh '''
               { set +x; } 2>/dev/null
               echo "Deleting project workspace..."
               cd /root/workspace && rm -r ${PROJ}
             '''
           } // steps
         }   // Deleting project workspace
  } // stages
}  // pipeline
