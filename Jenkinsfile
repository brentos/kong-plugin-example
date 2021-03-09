pipeline {

agent {
    label 'kong-plugin'
}
    environment {
        LUA_PATH="/kong/?.lua;/kong/?/init.lua;${WORKSPACE}/?.lua;${WORKSPACE}/?/init.lua;;"
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
