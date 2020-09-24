#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG="neil-provar-test@sonata-software.com"
    def SFDC_HOST = "https://login.salesforce.com"
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY="3MVG9n_HvETGhr3CEdo_aSlqljtoudIMupWRZtPIMQW0DWSef2Yf96lnmE_42pkK_2djCL_Sn4JZr7evz2_CL"

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    	stage('Authenticate Devhub') {
            sh "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} \
--jwtkeyfile /usr/JWT_salesforce/JWT/old/server.key --username ${HUB_ORG} \
--setdefaultdevhubusername --setalias myhuborg"
        }
        
        stage('Create Scratch Org') {
            // need to pull out assigned username
            rmsg = sh returnStdout: true, script: "\"${toolbelt}\" force:org:create -f config/project-scratch-def.json --json -s -a QAUbuntu"
            println(rmsg)
            def jsonSlurper = new JsonSlurperClassic()
            def robj = jsonSlurper.parseText(rmsg)
            if (robj.status != 0) { error 'org creation failed: ' + robj.message }
            SFDC_USERNAME=robj.result.username
            println(SFDC_USERNAME)
            robj = null
        }

    	stage('Set Default scratch org') {
            rc = sh returnStatus: true, script: "\"${toolbelt}\" force:config:set --global defaultusername=${SFDC_USERNAME} --json"
            if (rc != 0) { error 'Default scratch org failed' }
        }

        stage('Create password for scratch org') {
 			rmsg = sh returnStdout: true, script: "\"${toolbelt}\" force:user:password:generate --json"
			println(rmsg)
			def jsonSlurper = new JsonSlurperClassic()
			def robj = jsonSlurper.parseText(rmsg)
            if (robj.status != 0) { error 'password generation failed: ' + robj.message }
            robj = null
        }
}
