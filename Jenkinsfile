#!groovy
import groovy.json.JsonSlurperClassic
node {

def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG= env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
    
    def toolbelt = tool 'toolbelt'
    
    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
   

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    
    // println CONNECTED_APP_CONSUMER_KEY_TARGET
    // println SFDC_SERVER_KEY

    stage('Example Username/Password') {
        
    withCredentials([file(credentialsId: 'e06cb06c-72a2-44cb-a831-0c52d26c4ea9', variable: 'jwt_key_file')]) {
        println 'Inside With Credentials'
        
        stage('Authorize the Org'){
             // Login using JWT auth mechanism into the target instance and use credentials defined in the Global Credentials (unrestricted) 
            rc1 = command "${toolbelt}/sfdx force:auth:jwt:grant --clientid 3MVG9cHH2bfKACZZV5KqEsTf5X4IrB7hvKrUP9X_ig2OgvUc5bGvaNWPCI48UTONvQHk69_R5J9aF9Hq3ZgpX --jwtkeyfile ${jwt_key_file} --username naga@naga.devtest --instanceurl https://login.salesforce.com  --setdefaultdevhubusername --setalias HubOrg"
            if(rc1 != 0){
                error 'Salesforce Authorisation is failed.'
            }
                
        }
        stage('deploy code') {
            // Logout from previous authenticated connections to avoid connection errors from CLI
           // rc = command "${toolbelt}/sfdx force:auth:logout -u naga@naga.devtest -p"
                       
            // Deploy metadata
            rmsg = command "${toolbelt}/sfdx force:source:deploy -c -p ./force-app/main/ -u targetSandbox -l RunLocalTests"
            
            if(rmsg != 0){
                error 'Salesforce deploy failed.'
            }
            //Delete/Destroy Metadata
            //rmsg1 = command "${toolbelt}/sfdx force:mdapi:deploy -d destroy -u targetSandbox -w -1"
        }
    }
  }      
}
