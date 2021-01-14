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
    LABELS="${params.labels}"
    CONTEXTS="${params.contexts}"
    PATH="/Users/support.liquibase.net/liquibase:$PATH"
    LB_COMMAND="${params.command}"
  }
  stages {

    stage ('Checkout') {
      steps {
        // checkout Liquibase project from repo
        sh '''
          echo contexts: $CONTEXTS
          { set +x; } 2>/dev/null
          cd /Users/support.liquibase.net/workspace
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
          cd /Users/support.liquibase.net/workspace/${PROJ}
          liquibase --version
          echo "------------------------------------"
          echo "----------liquibase status----------"
          echo "------------------------------------"
          liquibase --classpath=/Users/support.liquibase.net/Drivers/ojdbc10.jar --url=${ORACLE_URL} --username=${ENVIRONMENT_STEP} --password=${PASSWORD} --changeLogFile=master.xml --contexts="${CONTEXTS}" --labels="${LABELS}" status --verbose
          echo "------------------------------------"
          echo "----------liquibase ${LB_COMMAND}----------"
          echo "------------------------------------"
          echo "liquibase --classpath=/Users/support.liquibase.net/Drivers/ojdbc10.jar --url=${ORACLE_URL} --username=${ENVIRONMENT_STEP} --password=${PASSWORD} --changeLogFile=master.xml --contexts=${CONTEXTS} --labels=${LABELS} ${LB_COMMAND}"
          liquibase --classpath=/Users/support.liquibase.net/Drivers/ojdbc10.jar --url=${ORACLE_URL} --username=${ENVIRONMENT_STEP} --password=${PASSWORD} --changeLogFile=master.xml --contexts=${CONTEXTS} --labels=${LABELS} ${LB_COMMAND}
        '''
      } // steps
    }   // Environment stage

        stage ('Deleting project workspace'){
           steps {
             sh '''
               { set +x; } 2>/dev/null
               echo "Deleting project workspace..."
               cd /Users/support.liquibase.net/workspace && rm -r ${PROJ}
             '''
           } // steps
         }   // Deleting project workspace
  } // stages
}  // pipeline
