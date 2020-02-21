#!groovy

@Library('globalPipelineLibraryMarkEWaite') _

pipeline {
    agent {
        label '!windows' // allow sh step
    }
    tools {
        ant 'ant-latest'
    }
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage("Checkout") {
            steps {
                echo "Branch is ${env.BRANCH_NAME}"
                echo "scm is ${scm}"
                echo "scm.userRemoteConfigs is ${scm.userRemoteConfigs}"
                echo "scm.userRemoteConfigs[0].url is ${scm.userRemoteConfigs[0].url}"
                sh "env | sort"
                checkout(
                  [ $class: 'GitSCM',
                    branches: [[name: "refs/heads/${env.BRANCH_NAME}"]],
                    extensions: [
                      [ $class: 'CloneOption', depth: 1, honorRefspec: true, noTags: true, reference: '/var/lib/;git/mwaite/bugs/jenkins-bugs.git', shallow: true],
                      [ $class: 'LocalBranch', localBranch: "${env.BRANCH_NAME}"],
                      [ $class: 'PruneStaleBranch']
                    ],
                    gitTool: scm.gitTool,
                    userRemoteConfigs: [
                      [ refspec: "+refs/heads/${env.BRANCH_NAME}:refs/remotes/origin/${env.BRANCH_NAME} +refs/heads/pr/*:refs/remotes/origin/pr/*",
                        url: scm.userRemoteConfigs[0].url
                      ]
                    ]
                  ]
                )
                sh "ls -alt"
            }
        }
    }
}
