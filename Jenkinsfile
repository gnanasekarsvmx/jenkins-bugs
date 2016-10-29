#!groovy

/* Only keep the 10 most recent builds. */
properties([[$class: 'BuildDiscarderProperty',
                strategy: [$class: 'LogRotator', numToKeepStr: '10']]])

node("git-1.8+ && !windows") { // Windows garbles Japanese commit text
  stage('Checkout') {
    checkout scm
  }

  stage('Build') {
    /* Call the ant build. */
    def repo = "${env.JENKINS_URL}/userContent.git"
    def step = fileLoader.fromGit('pipelineSteps', "${repo}", 'master', null, '')
    step.ant "increment"
  }

  stage('Verify') {
    // Does not check the actual bug report
    // Bug report was that recent changes are not in the recent changes list
    // This checks that the log writes Japanese, not that the recent changes do
    def repo = "${env.JENKINS_URL}/userContent.git"
    def check = fileLoader.fromGit('pipelineChecks', "${repo}", 'master', null, '')
    check.logContains(".*ビルド番号をインクリメント.*", "Missing localized text 1.")
    check.logContains(".*でした.*", "Missing localized text 2.")
  }
}
