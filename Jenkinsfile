import groovy.json.JsonSlurper

pipeline {
    agent any

    stages {
        stage('Update Status') {
            steps {
                script {
                    
                    // Define the issueKey by Commit Message
                    def key = sh(script: "git log --format=%B -n 1 HEAD", returnStdout: true).trim()
                    
                    def index = key.indexOf(' ')
                    def issueKey = key.substring(0, index)

                    if (issueKey) {
                        // Define the new status
                        def newStatus = "Done"
                        
                        // Update status using REST API
                        def transitionId = getTransitionId(issueKey, newStatus)
                        println "Transition ID for ${newStatus}: ${transitionId}"

                        if (transitionId != null) {
                            def response = sh(script: "curl -u <jira username:password> -X POST -H 'Content-Type: application/json' -d '{\"transition\": {\"id\": \"${transitionId}\"}}' http://<public_ip>:<jira_port>/rest/api/2/issue/${issueKey}/transitions", returnStdout: true)
                            println "Response: ${response}"
                        } else {
                            println "Transition ID not found for ${newStatus}."
                        }
                    } else {
                        println "Issue key not found in pull request title."
                    }
                }
            }
        }
    }
}

def getTransitionId(issueKey, statusName) {
    def response = sh(script: "curl -u <jira username:password> -X GET -H 'Content-Type: application/json' http://<public_ip>:<jira_port>/rest/api/2/issue/${issueKey}/transitions", returnStdout: true).trim()
    def jsonSlurper = new JsonSlurper()
    def transitions = jsonSlurper.parseText(response)

    println "All Transitions: ${transitions}"

    for (transition in transitions.transitions) {
        println "Transition: ${transition}"
        if (transition.to.name == statusName) {
            return transition.id
        }
    }

    return null
}
