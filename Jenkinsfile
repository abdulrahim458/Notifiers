#!groovy
//Test
pipeline {

    agent {
        label 'general-purpose'
    }
    tools {
        jdk "zulu-8"
        maven "maven-3.6.3"
    }
    environment {
        JAVA_HOME = tool("zulu-11")
        MAVEN_OPTS = '-Xmx2G -Djavax.net.ssl.trustStore=${JAVA_HOME}/jre/lib/security/cacerts'
        payaraBuildNumber = "${BUILD_NUMBER}"
    }
    stages {

        stage('Checkout Payara5-Enterprise') {
            steps {
                script {
                    checkout changelog: false, poll: true, scm: [$class: 'GitSCM',
                    branches: [[name: "enterprise"]],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [], 
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: 'payara-devops-github-personal-access-token-as-username-password', url:"https://github.com/payara/Notifiers.git"]]]
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo '*#*#*#*#*#*#*#*#*#*#*#*#  Building SRC  *#*#*#*#*#*#*#*#*#*#*#*#*#*#*#'
                    sh """mvn -B -V -ff -e clean install --strict-checksums \
                        -Djavax.net.ssl.trustStore=${env.JAVA_HOME}/lib/security/cacerts \
                        -Djavax.xml.accessExternalSchema=all -Dbuild.number=${payaraBuildNumber} \
                        -Djavadoc.skip -Dsource.skip"""
                    echo '*#*#*#*#*#*#*#*#*#*#*#*#    Built SRC   *#*#*#*#*#*#*#*#*#*#*#*#*#*#*#'
                }
            }
        }
    }
}
