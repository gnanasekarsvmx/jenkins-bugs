pipeline {
  agent none
  options {
    durabilityHint('MAX_SURVIVABILITY') // Should be most disc intensive
    buildDiscarder logRotator(numToKeepStr: '60')
  }
  stages {
    stage("JENKINS-59785 ${GIT_COMMIT}") {
      parallel {
        stage("windows ${BUILD_NUMBER}") {
          agent { 
            label 'windows'
          }
          when {
            anyOf {
              changeset "LICENSE"
              changeset "README*"
              changeset "build*l"
              changeset "*.xml"
              changeset "build*r"
              changeset "build.*"
              changeset "*.number"
              changeset "*num*r"
            }
          }
          steps {
            echo 'Workspace before windows ws is ' + WORKSPACE
            ws(dir: WORKSPACE + '/windows-dir') {
              echo 'Workspace inside windows ws is ' + WORKSPACE
            }
            bat 'echo hello windows from %COMPUTERNAME%'
            bat 'if not exist build.xml exit 1'
          }
        }
        stage("unix ${BUILD_ID}") {
          agent {
            label '!windows'
          }
          when {
            anyOf {
              changeset "LICENSE"
              changeset "README*"
              changeset "R*md"
              changeset "RE*md"
              changeset "REA*md"
              changeset "READ*md"
              changeset "READM*md"
              changeset "README.md"
              changeset "*.xml"
              changeset "*.number"
              changeset "*.n*r"
            }
          }
          steps {
            echo 'Workspace before unix ws is ' + WORKSPACE
            ws(dir: WORKSPACE + '/unix-dir') {
              echo 'Workspace inside unix ws is ' + WORKSPACE
            }
            sh 'echo hello unix from `hostname`; ls build.xml'
            sh '[ -f build.xml ] || exit 1'
          }
        }
      }
    }
  }
}
