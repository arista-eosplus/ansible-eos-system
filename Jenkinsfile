#!/usr/bin/env groovy

/*
 * Jenkinsfile for ansible-eos-ipv4 role
 *
 * Run the Ansible-Role-Test job against a commit or
 * pull request in the ansible-eos-ipv4 repo
 */

// Change this comment to force a run of the pipeline  02

pipeline {
    agent{ label 'jenkins-exec-02'}
    options {
        buildDiscarder(
            // Only keep the 10 most recent builds
            logRotator(numToKeepStr:'10'))
    }
    environment {
        projectName = 'ansible-eos-ipv4'
        emailTo = 'grybak@arista.com'
        emailFrom = 'grybak+jenkins@arista.com'
    }

    stages {
        stage ('Run tests for ansible-eos-ipv4 role') {
            steps {
                    build job: 'gar-test-starter',
                          parameters: [string(name: 'ROLE_NAME', value: 'ansible-eos-ipv4')]
            }
            when {
                // Only run against 'master' branch
                branch 'master'
            }
        }
    }

    post {
        failure {
            // Send an email with a link to logs on failure
            mail to: env.emailTo,
                 from: env.emailFrom,
                 subject: "${env.projectName} ${env.JOB_NAME} (${env.BUILD_NUMBER}) build failed",
                 body: "${env.JOB_NAME} (${env.BUILD_NUMBER}) ${env.projectName} build error " +
                       "is here: ${env.BUILD_URL}\nStarted by ${env.BUILD_CAUSE}"
        }
        success {
            // Send an email notification on success
            mail to: env.emailTo,
                 from: env.emailFrom,
                 subject: "${env.projectName} ${env.JOB_NAME} (${env.BUILD_NUMBER}) build successful",
                 body: "${env.JOB_NAME} (${env.BUILD_NUMBER}) ${env.projectName} build successful\n" +
                       "Started by ${env.BUILD_CAUSE}\n" +
                       "${env.BUILD_URL}"
        }
    }
}
