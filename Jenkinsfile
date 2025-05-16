pipeline {
    agent any
    environment {
      PRODUCTS_DB_IMAGE = "${REGISTRY}/${REPOSITORY}/products-db-replicated:v1-${BUILD_NUMBER}"
      PRODUCTS_API_IMAGE = "${REGISTRY}/${REPOSITORY}/products-api:v1-${BUILD_NUMBER}"
      STOCK_API_IMAGE = "${REGISTRY}/${REPOSITORY}/stock-api:v1-${BUILD_NUMBER}"
      WEB_IMAGE = "${REGISTRY}/${REPOSITORY}/web:v2-${BUILD_NUMBER}"
    }
    stages {
        stage('Audit tools') {                        
            steps {
                sh '''
                  git version
                  buildctl --version
                  kubectl version --short
                  helm version --short
                '''
            }
        }
        stage('Build & push database') {
            steps {
              echo "Building image tag: ${PRODUCTS_DB_IMAGE}"
              sh '''
                buildctl --addr tcp://buildkitd:1234 \
                  build --frontend=dockerfile.v0 \
                  --local context=./hackathon/project/src/db/postgres-replicated \
                  --local dockerfile=./hackathon/project/src/db/postgres-replicated \
                  --output type=image,name="${PRODUCTS_DB_IMAGE}",push=true
              '''
            }
        }
        stage('Build & push products API') {
            steps {
              echo "Building image tag: ${PRODUCTS_API_IMAGE}"
              sh '''
                buildctl --addr tcp://buildkitd:1234 \
                  build --frontend=dockerfile.v0 \
                  --local context=./hackathon/project/src/products-api/java \
                  --local dockerfile=./hackathon/project/src/products-api/java \
                  --output type=image,name="${PRODUCTS_API_IMAGE}",push=true
              '''
            }
        }
        stage('Build & push stock API') {
            steps {
              echo "Building image tag: ${STOCK_API_IMAGE}"
              sh '''
                buildctl --addr tcp://buildkitd:1234 \
                  build --frontend=dockerfile.v0 \
                  --local context=./hackathon/project/src/stock-api/golang \
                  --local dockerfile=./hackathon/project/src/stock-api/golang \
                  --output type=image,name="${STOCK_API_IMAGE}",push=true
              '''
            }
        }
        stage('Build & push web') {
            steps {
              echo "Building image tag: ${WEB_IMAGE}"
              sh '''
                buildctl --addr tcp://buildkitd:1234 \
                  build --frontend=dockerfile.v0 \
                  --local context=./hackathon/project/src/web/dotnet \
                  --local dockerfile=./hackathon/project/src/web/dotnet \
                  --output type=image,name="${WEB_IMAGE}",push=true
              '''
            }
        }        
        stage('Deploy to test namespace') {
            steps {
                sh '''
                  # upgrade or install using the newly-pushed images:
                  helm upgrade --install \
                    --namespace widg-smoke \
                    --create-namespace \
                    -f hackathon/files/helm/smoke-test.yaml \
                    --set pdbImage=${PRODUCTS_DB_IMAGE} \
                    --set papiImage=${PRODUCTS_API_IMAGE} \
                    --set sapiImage=${STOCK_API_IMAGE} \
                    --set webGreenImage=${WEB_IMAGE} \
                    widg-smoke \
                    hackathon/solution-part-7/helm/widgetario/

                  # list releases to show the output in Jenkins:
                  helm ls -n widg-smoke
                '''
            }
        }
    }
}
