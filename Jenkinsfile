#!/usr/bin/env groovy

pipeline {
    agent {
        label 'os:linux && terraform'
    }
    options {
        skipDefaultCheckout()
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'RELEASE_VERSION',
            defaultValue: '',
            description: 'The version of the terraform provider. (MAJOR.MINOR.PATCH, e.g. 0.0.1). If the version is left empty, release stage will be skipped.',
            trim:true)
    }
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }
        stage("Build") {
            when {
                expression {
                    params.RELEASE_VERSION == ''
                }
            }
            steps {
                goreleaser goVersion: '1.20.5', snapshot: true
                archiveArtifacts artifacts: 'dist/*.zip', fingerprint: true
            }
        }
        stage("Release") {
            when {
                expression {
                    params.RELEASE_VERSION != ''
                }
            }
            steps {
                goreleaser releaseVersion: params.RELEASE_VERSION
            }
        }
    }
}
