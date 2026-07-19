// Configure jenkins CI/CD pipeline
pipeline {
    agent any

    environment {                                                   // fixed: was "environemnt" (typo)
        BACKEND_IMAGE  = "sumeruc/todo-backend:${BUILD_NUMBER}"
        FRONTEND_IMAGE = "sumeruc/todo-frontend:${BUILD_NUMBER}"
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/chougulesumeru/TaskFLow-k8s.git',
                    credentialsId: 'git-cred'
            }
        }

        stage('Build backend image') {
            steps {
                sh "docker build -t ${BACKEND_IMAGE} ./src/backend"   // fixed: added -t flag; removed duplicate ${BUILD_NUMBER}; corrected path
            }
        }

        stage('Build frontend image') {
            steps {
                sh "docker build -t ${FRONTEND_IMAGE} ./src/frontend" // fixed: added -t flag; removed duplicate ${BUILD_NUMBER}; corrected path
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                        docker push ${BACKEND_IMAGE}
                        docker push ${FRONTEND_IMAGE}
                    """
                }
            }
        }

        // ── Kubernetes CD ────────────────────────────────────────────────

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'k8s-cred']) {

                    // 1. Namespace + config (idempotent — safe to re-apply)
                    sh 'kubectl apply -f k8s/namespace.yml'
                    sh 'kubectl apply -f k8s/configmap.yml'
                    sh 'kubectl apply -f k8s/secret.yml'

                    // 2. Storage
                    sh 'kubectl apply -f k8s/mongodb_pvc.yml'

                    // 3. MongoDB
                    sh 'kubectl apply -f k8s/mongodb_deployment.yml'
                    sh 'kubectl apply -f k8s/mongodb_service.yml'

                    // 4. Patch backend + frontend images to the current build tag
                    sh "kubectl set image deployment/backend-deployment  backend=${BACKEND_IMAGE}  -n todo-app"
                    sh "kubectl set image deployment/frontend-deployment frontend=${FRONTEND_IMAGE} -n todo-app"

                    // 5. Services + Ingress
                    sh 'kubectl apply -f k8s/backend_service.yml'
                    sh 'kubectl apply -f k8s/frontend_service.yml'
                    sh 'kubectl apply -f k8s/ingress.yml'

                    // 6. HPA (needs deployments to exist first)
                    sh 'kubectl apply -f k8s/hpa.yml'
                }
            }
        }

        stage('Verify Rollout') {
            steps {
                withKubeConfig([credentialsId: 'k8s-cred']) {
                    // Block pipeline until both deployments are fully up
                    sh 'kubectl rollout status deployment/backend-deployment  -n todo-app --timeout=120s'
                    sh 'kubectl rollout status deployment/frontend-deployment -n todo-app --timeout=120s'
                    sh 'kubectl rollout status deployment/mongo-deployment     -n todo-app --timeout=120s'
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded. Images deployed: ${BACKEND_IMAGE} | ${FRONTEND_IMAGE}"
        }
        failure {
            echo 'Pipeline failed. Check stage logs above.'
        }
        always {
            // Clean up local docker images to free disk space on the agent
            sh "docker rmi ${BACKEND_IMAGE}  || true"
            sh "docker rmi ${FRONTEND_IMAGE} || true"
        }
    }
}
