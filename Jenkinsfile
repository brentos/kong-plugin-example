pipeline {

agent {
    label 'kong-plugin'
}

    stages{
    stage('Preparation') { // for display purposes
    steps {
        // Get some code from a GitHub repository
        git branch: 'main', url: 'https://github.com/brentos/kong-plugin-example'
    }

    }
    stage('Build') {
        steps{
        // Run the maven build
        sh 'luacheck .'
        sh 'whoami'

        sh 'cd /kong && bin/busted $WORKSPACE/spec'
        sh 'luarocks make --pack-binary-rock'
        }
    }
    stage('Results') {
        steps {
        archiveArtifacts '*.rock'
        }
    }
    }
}
