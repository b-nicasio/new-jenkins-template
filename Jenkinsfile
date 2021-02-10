def TIMESTAMP = Calendar.getInstance().getTime().format('YYYYMMdd-hhmm')

pipeline {
  agent {
    label "master"
  }

  environment {
    AGENT_NAME            = "new-template"
    STAGE_AGENT_URL       = "https://cocky-jennings-177ffb.netlify.app/"
    PRODUCTION_AGENT_URL  = "https://cocky-jennings-177ffb.netlify.app/"
  }

  stages {
    stage("Initialize") {
      steps {
        script {
          if (env.BRANCH_NAME == 'master') {
            timeout (time: 15, unit: 'MINUTES') {
              input message: 'Are you sure you want to deploy the agent to production? (Click "Proceed" to continue...)'
            }
            sh(
              label: "Run production agent deploy",
              script: """
                cd /srv/deployment/remax/stage
                ansible-playbook agent.yml --extra-vars "agent_name=${AGENT_NAME} agent_url=${PRODUCTION_AGENT_URL}"
              """
            )
          }
          if (env.BRANCH_NAME == 'dev') {
            sh(
              label: "Run stage agent deploy",
              script: """
                cd /srv/deployment/remax/stage
                ansible-playbook agent.yml --extra-vars "agent_name=${AGENT_NAME} agent_url=${STAGE_AGENT_URL}"
              """
            )
          }
          else {
              echo 'Nothing to deploy here'
          }
        }
      }
    }
  }
}
