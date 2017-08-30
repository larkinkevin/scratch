#!/usr/bin/env groovy

pipeline {
    agent {
        docker {
            label "docker"
            registryUrl "https://docker-registry.pdbld.f5net.com"
            image "bdo/jenkins-worker-ubuntu-16.04:master"
            args "-v /etc/localtime:/etc/localtime:ro" \
                + " -v /srv/mesos/trtl/results:/home/jenkins/results" \
                + " -v /srv/nfs:/testlab" \
                + " --env-file /srv/kubernetes/infra/jenkins-worker/config/velcro-test.env"
        }
    }
    options {
        ansiColor('xterm')
        timestamps()
        timeout(time: 5, unit: "MINUTES")
    }
    stages {
        stage("systest") {
            steps {
                sh '''#!/usr/bin/env bash
                    # - print env vars
                    printenv | sort | grep -v OS_PASSWORD
                '''
            }
        }
    }
    post {
        always {
            // cleanup workspace
            dir("${env.WORKSPACE}") { deleteDir() }
        }
    }
}
