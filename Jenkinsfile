#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    //gitlabCommitStatus('build') {
        stage('check') {
            bat "echo $PATH"
            bat "java -version"
            bat "mvn -version"
            bat "gradle -version"
            bat "./gradlew -version"
            bat "node --version"
            bat "yarn --version"
        }

        stage('clean') {
            bat "chmod +x gradlew"
            bat "./gradlew clean --no-daemon"
        }

        stage('install tools') {
            bat "./gradlew yarn_install -PnodeInstall --no-daemon"
        }

        stage('backend tests') {
            try {
                bat "./gradlew test -PnodeInstall --no-daemon"
            } catch(err) {
                throw err
            } finally {
                junit '**/build/**/TEST-*.xml'
            }
        }

        stage('frontend tests') {
            try {
                bat "./gradlew yarn_test -PnodeInstall --no-daemon"
            } catch(err) {
                throw err
            } finally {
                junit '**/build/test-results/karma/TESTS-*.xml'
            }
        }

        stage('packaging') {
            bat "./gradlew bootRepackage -x test -Pprod -PnodeInstall --no-daemon"
            archiveArtifacts artifacts: '**/build/libs/*.war', fingerprint: true
        }

        /*stage('quality analysis') {
            withSonarQubeEnv('Sonar') {
                bat "./gradlew sonarqube --no-daemon"
            }
        }*/
    //}
}
