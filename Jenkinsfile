#!groovy

import groovy.json.JsonSlurperClassic

node {
    // Environment Variables
    def SF_CONSUMER_KEY = env.SF_CONSUMER_KEY
    def SF_USERNAME = env.SF_USERNAME
    def SERVER_KEY_CREDENTALS_ID = env.SERVER_KEY_CREDENTALS_ID
    def TEST_LEVEL = 'RunLocalTests'
    def PACKAGE_NAME = '0Ho1U000000CaUzSAK'
    def PACKAGE_VERSION
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://login.salesforce.com"
    def toolbelt = tool 'toolbelt' // Custom tool pointing to /usr/local/bin/sfdx

    // Check Salesforce CLI Version
    stage('Check SFDX CLI Version') {
        echo 'Checking Salesforce CLI (SFDX) version...'
        def versionOutput = sh(returnStdout: true, script: "${toolbelt}/sf --version").trim()
        echo "SFDX CLI Version: ${versionOutput}"
    }

    // Checkout Source Code
    stage('Checkout Source') {
        checkout scm
    }

    // Use Salesforce JWT credentials
    withEnv(["HOME=${env.WORKSPACE}"]) {
        withCredentials([file(credentialsId: SERVER_KEY_CREDENTALS_ID, variable: 'server_key_file')]) {

            // Authorize Dev10 Org
            stage('Authorize Dev10 Org') {
                sh """
                    ${toolbelt}/sf org login jwt --instance-url ${SF_INSTANCE_URL} --client-id ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwt-key-file ${server_key_file} --no-prompt
                """
            }

            // Additional stages can go here...
        }
    }
}

// Helper Methods
def executeCommand(command, errorMessage) {
    if (command("${command}") != 0) {
        error errorMessage
    }
}
