#!/usr/bin/env groovy
pipeline {
  agent none
  stages {
    stage('Nexus IQ evaluation') {
      agent any
      steps {
        sh "yarn install --frozen-lockfile --cache-folder ${JENKINS_HOME}/.cache/yarn/${EXECUTOR_NUMBER}"
        sh "mvn clean package com.sonatype.clm:clm-maven-plugin:index -DskipTests -Dmaven.repo.local=${JENKINS_HOME}/.m2/repository-${EXECUTOR_NUMBER}"

        nexusPolicyEvaluation iqApplication: selectedApplication('opp'),
          iqStage: 'build',
          enableDebugLogging: false,
          failBuildOnNetworkError: false,
          iqModuleExcludes: [[moduleExclude: '**/internal-maven-plugin/target/**']],
          iqScanPatterns: [[scanPattern: '**/package.json']]
      }
    }
  }
}
