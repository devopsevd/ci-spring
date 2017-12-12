node("BuildServer") {
  def project = 'ci-spring-demo'
  def appName = 'ci-spring-demo'
  def imageTag = "devopsevd/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
  def mvnHome = tool 'Maven'
  
  checkout scm

  switch (env.BRANCH_NAME) {
    case "master":
        echo "master branch"
        stage("Master: Build & Unit Test"){   
            sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package cobertura:cobertura -Dcobertura.report.format=xml"
        }
        stage("Master: Code Scan"){      
            def scannerHome = tool 'SonarQubeScanner'
            withSonarQubeEnv('Sonar') { 
                if (isUnix()) {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Unit Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
            step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/target/site/cobertura/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
        }
        stage ("Master: Build image"){
            echo "imageTag: ${imageTag}"
            sh("docker build -t ${imageTag} .")

        }
        stage ("Master: Push image to registry"){
            //sh("docker login -u # -p #")
            sh("docker push ${imageTag}")
        }
        break


    default:
        echo "${env.BRANCH_NAME} branch"
        stage("${env.BRANCH_NAME}: Unit Test"){ 
            sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean cobertura:cobertura -Dcobertura.report.format=xml"
        }
        stage("${env.BRANCH_NAME}: Code Scan"){ 
            def scannerHome = tool 'SonarQubeScanner'
            withSonarQubeEnv('Sonar') { 
                if (isUnix()) {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Unit Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
        }
    }
 }
