node("JenkinsMasterDocker") {
  def project = 'spin-kub-demo'
  def appName = 'spin-kub-demo'
  def feSvcName = "${appName}-frontend"
  def imageTag = "devopsevd/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"

  checkout scm

  switch (env.BRANCH_NAME) {
    // Roll out to canary environment
    case "canary":
        // Change deployed image in canary to the one we just built
        //sh("sed -i.bak 's#devopsevd/gceme:1.0.0#${imageTag}#' ./k8s/canary/*.yaml")
        //sh("kubectl --namespace=production apply -f k8s/services/")
        //sh("kubectl --namespace=production apply -f k8s/canary/")
        //sh("echo http://`kubectl --namespace=production get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
        break

    // Roll out to production
    case "master":
        // Change deployed image in canary to the one we just built
        //sh("sed -i.bak 's#devopsevd/gceme:1.0.0#${imageTag}#' ./k8s/production/*.yaml")
        //sh("kubectl --namespace=production apply -f k8s/services/")
        //sh("kubectl --namespace=production apply -f k8s/production/")
        //sh("echo http://`kubectl --namespace=production get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
        echo "master branch"
        stage("Master: Unit Test"){          
        }
        stage("Master: Code Scan"){          
        }
        stage("Master: Code Coverage"){          
        }
        stage ("Master: Build image"){
              //sh("docker build -t ${imageTag} .")
              echo "imageTag: ${imageTag}"
        }
        stage ("Master: Run Go tests"){
             //sh("docker run ${imageTag} go test")
        }
        stage ("Master: Push image to registry"){
            //sh("docker login -u devopsevd -p Gapple@123")
            //sh("docker push ${imageTag}")
        }
        break

    // Roll out a dev environment
    default:
        // Create namespace if it doesn't exist
        //sh("kubectl get ns ${env.BRANCH_NAME} || kubectl create ns ${env.BRANCH_NAME}")
        // Don't use public load balancing for development branches
        //sh("sed -i.bak 's#LoadBalancer#ClusterIP#' ./k8s/services/frontend.yaml")
        //sh("sed -i.bak 's#devopsevd/gceme:1.0.0#${imageTag}#' ./k8s/dev/*.yaml")
        //sh("kubectl --namespace=${env.BRANCH_NAME} apply -f k8s/services/")
        //sh("kubectl --namespace=${env.BRANCH_NAME} apply -f k8s/dev/")
        //echo 'To access your environment run `kubectl proxy`'
        echo "${env.BRANCH_NAME} branch"
        stage("${env.BRANCH_NAME}: Unit Test"){          
        }
        stage("${env.BRANCH_NAME}: Code Scan"){          
        }
    }
 }
