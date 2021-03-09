pipeline {

    agent {
        label 'kong-plugin'
    }
    environment {
        PATH = "/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin:/usr/local/openresty/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/luajit/bin:/usr/local/go/bin"
        LUA_PATH = "/kong/?.lua;/kong/?/init.lua;${WORKSPACE}/?.lua;${WORKSPACE}/?/init.lua;;"
        KONG_PREFIX = "/kong/servroot"
        KONG_ADMIN_LISTEN = "0.0.0.0:8001"
        KONG_ADMIN_LISTEN_SSL = "0.0.0.0:8444"
        TEST_NGINX_NXSOCK = "/tmp"
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
                sh 'scp kong-plugin-myplugin-0.1.0-1.all.rock kong@ec2-34-217-208-29.us-west-2.compute.amazonaws.com:/tmp/'
                sh 'ssh kong@ec2-34-217-208-29.us-west-2.compute.amazonaws.com "luarocks install kong-plugin-myplugin-0.1.0-1.all.rock && rm /tmp/kong-plugin-myplugin-0.1.0-1.all.rock"'
            }
        }
    }
}
