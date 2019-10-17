#!/usr/bin/env groovy

import java.net.URL


node{
    stage('Git Checkout'){
        git 'https://github.com/rhkaru/DevOpsClassCodes.git'
        //can have any name for 'Git Checkout' 
    }
    stage('Compile'){
        withMaven(maven:'mymaven'){
            sh 'mvn compile'
        }
    }
    stage('Review'){
        try{
            withMaven(maven:'mymaven'){
                sh 'mvn pmd:pmd'
            }
        } finally {
            pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
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
    stage('Coverage check'){
         try{
            withMaven(maven:'mymaven'){
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
        } finally{
            cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
        } 
        
    }
    stage('Package'){
        withMaven(maven:'mymaven'){
            sh 'mvn package'
        }
    stage ('build-docker-image'){
          sh 'cp /var/lib/jenkins/workspace/MyPipeline/target/addressbook.war .; sudo docker build . -t rhkaru/addressbook:$BUILD_NUMBER; sudo docker push rhkaru/addressbook:$BUILD_NUMBER'  
          
           
    }    
    }
   
}
