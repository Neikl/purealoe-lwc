#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG="neil.lohit@sonata-software.com"
    def SFDC_HOST = "https://login.salesforce.com"
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY="3MVG9n_HvETGhr3DySO6mrYFt7w.WztmP9q4mLCK.WSa0VzfQYpxpwAsepaKcjUPG3hPxq5z6IRqNiAt2otjJ"

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    	stage('Authenticate Devhub') {
            sh "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} \
--jwtkeyfile /usr/JWT_salesforce/JWT/server.key --username ${HUB_ORG} \
--setdefaultdevhubusername --setalias myhuborg"
        }
        
        stage('Create Scratch Org') {
            // need to pull out assigned username
            sh "sfdx force:org:create -f config/project-scratch-def.json -a DevUbuntu"
        }
}
