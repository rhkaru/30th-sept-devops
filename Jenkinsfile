#!/usr/bin/env groovy

import java.net.URL

node{
    stage('Git Checkout'){
        git 'https://github.com/npsoni88/DevOpsClassCodes.git'
    }
    stage('Compile'){
        withMaven(maven:'mymaven'){
            sh 'mvn compile'
        }
    }
    stage('Test'){
        try{
            withMaven(maven:'mymaven'){
                sh 'mvn test'
            }
        } finally{
            junit 'target/surefire-reports/*.xml'
        } 
    }
    stage('Package'){
        withMaven(maven:'mymaven'){
            sh 'mvn package'
        }
    }
   
}
