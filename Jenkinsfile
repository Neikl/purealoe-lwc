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
            sh "sfdx force:auth:jwt:grant --clientid 3MVG9n_HvETGhr3CEdo_aSlqljtoudIMupWRZtPIMQW0DWSef2Yf96lnmE_42pkK_2djCL_Sn4JZr7evz2_CL \
--jwtkeyfile /home/osboxes/Nekl/JWT/server.key --username neil-provar-test@sonata-software.com \
--setdefaultdevhubusername --setalias myhuborg"
        }
        
        stage('Create Scratch Org') {
            // need to pull out assigned username
            rmsg = sh returnStdout: true, script: "\"${toolbelt}\" force:org:create -f config/developerOrg-scratch-def.json --json -s -a df13@makepositive.com"
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
}
