#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    //gitlabCommitStatus('build') {
        stage('check version') {
            bat "echo $PATH"
            bat "java -version"
            bat "mvn -version"
            bat "gradle -version"
            bat "./gradlew -version"
            bat "node --version"
            bat "yarn --version"
        }

//        stage('clean') {
//            bat "chmod +x gradlew"
//            bat "./gradlew clean --no-daemon"
//        }
//
//        stage('install tools') {
//            bat "./gradlew yarn_install -PnodeInstall --no-daemon"
//        }
//
//        stage('backend tests') {
//            try {
//                bat "./gradlew test -PnodeInstall --no-daemon"
//            } catch(err) {
//                throw err
//            } finally {
//                junit '**/build/**/TEST-*.xml'
//            }
//        }
//
//        stage('frontend tests') {
//            try {
//                bat "./gradlew yarn_test -PnodeInstall --no-daemon"
//            } catch(err) {
//                throw err
//            } finally {
//                junit '**/build/test-results/karma/TESTS-*.xml'
//            }
//        }
//
//        stage('packaging') {
//            bat "./gradlew bootRepackage -x test -Pprod -PnodeInstall --no-daemon"
//            archiveArtifacts artifacts: '**/build/libs/*.war', fingerprint: true
//        }

        stage('deploy') {
            def server = Artifactory.server 'ARTIFACTORY-CLOUD'

            //def rtGradle = Artifactory.newGradleBuild()

            //rtGradle.deployer server: server, repo: 'libs-release-local'

            //rtGradle.deployer.artifactDeploymentPatterns.addInclude("*.war").addInclude("*.jar")

            //rtGradle.deployer.addProperty("status", "in-int")

            bat "pwd"

            def uploadSpec = """ {
              "files": [
                {
                  "pattern": "build/libs/tjj-0.0.1-SNAPSHOT.war",
                  "target": "libs-release-local/",
                  "props": "status=in-int"
                }
             ]
            }"""
            def buildInfo = server.upload(uploadSpec)

            server.publishBuildInfo(buildInfo)

        }

        /*stage('quality analysis') {
            withSonarQubeEnv('Sonar') {
                bat "./gradlew sonarqube --no-daemon"
            }
        }*/
    //}
}
