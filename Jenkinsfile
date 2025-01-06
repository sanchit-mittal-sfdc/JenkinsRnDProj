node {
    def SF_CONSUMER_KEY = env.SF_CONSUMER_KEY
    def SF_USERNAME = env.SF_USERNAME
    def SERVER_KEY_CREDENTALS_ID = env.SERVER_KEY_CREDENTALS_ID
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://login.salesforce.com"
    
    def toolbelt = tool 'toolbelt' // Ensure toolbelt points to SFDX path

    // Stage to check Salesforce CLI version
    stage('Check SFDX CLI Version') {
        echo "Checking Salesforce CLI version..."
        def result = command("${toolbelt}/sf --version")
        if (result != 0) {
            error "Failed to retrieve Salesforce CLI version."
        }
    }

    // Stage to authenticate DevHub using JWT
    withCredentials([file(credentialsId: SERVER_KEY_CREDENTALS_ID, variable: 'server_key_file')]) {
        stage('Authenticate DevHub') {
            echo "Authenticating Salesforce DevHub using JWT..."
            def result = command("${toolbelt}/sf org login jwt --instance-url ${SF_INSTANCE_URL} --client-id ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwt-key-file ${server_key_file} --set-default-dev-hub --alias HubOrg")
            if (result != 0) {
                error "Salesforce DevHub authentication failed."
            }
        }
    }
}

// Utility function to handle platform-specific command execution
def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
        return bat(returnStatus: true, script: script);
    }
}
