#!groovy

// Jenkinsfile based check not feasible, since this requires a dedicated
// job configured with the expected build chooser

@Library('globalPipelineLibraryMarkEWaite')
import com.markwaite.Assert
import com.markwaite.Build

/* Only keep the 7 most recent builds. */
properties([[$class: 'BuildDiscarderProperty',
             strategy: [$class: 'LogRotator', numToKeepStr: '7']]])

def branch="master"

node('master') {

  stage('Checkout') {
    checkout([$class: 'GitSCM',
              userRemoteConfigs: [[url: 'https://bitbucket.org/markewaite/git-client-plugin.git',
                                   name: 'origin',
                                   refspec: "+refs/heads/${branch}:refs/remotes/origin/${branch}",
                                  ]],
              branches: [[name: "*/${branch}"]],
              browser: [$class: 'GithubWeb',
                        repoUrl: 'https://bitbucket.org/markewaite/git-client-plugin.git'],
              extensions: [[$class: 'AuthorInChangelog'],
                           [$class: 'CheckoutOption', timeout: 1],
                           [$class: 'CleanCheckout'],
                           [$class: 'CloneOption',
                            depth: 2,
                            honorRefspec: true,
                            noTags: true,
                            reference: '/var/lib/git/mwaite/jenkins/git-client-plugin.git',
                            shallow: true,
                            timeout: 3],
                           [$class: 'LocalBranch', localBranch: '**'],
                           [$class: 'PruneStaleBranch'],
                           ],
             ])
  }

  stage('Build') {
    /* Call the maven build. */
    def step = new com.markwaite.Build()
    step.maven "clean"
  }

  stage('Verify') {
    def check = new com.markwaite.Assert()
    check.logContains("maven clean", "Maven clean not found")
  }

}
