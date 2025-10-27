pipeline {
  agent any

  environment {
    GIT_URL = "https://github.com/aLieexe/nfa-case-study-4.git"
    GIT_BRANCH = "main"
    IMAGE = "alie12/nfa-case4"
    TAG = "latest"
    DOCKER_CRED = "docker-hub"
    KUBECONFIG_CRED = "kubeconfig-dev"
    NAMESPACE = "default"
    HELM_RELEASE = "casestudy-jenkins1"
  }

  stages {
    stage('Checkout Source Code') {
      steps {
        git url: "${GIT_URL}", branch: "${GIT_BRANCH}"
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          echo "🛠️ Building image ${IMAGE}:${TAG}..."
          def builtImage = docker.build("${IMAGE}:${TAG}")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: "docker-hub",
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          script {
            echo "📦 Pushing image to DockerHub..."
            sh """
              echo "$PASS" | docker login -u "$USER" --password-stdin
              docker push ${IMAGE}:${TAG}
            """
          }
        }
      }
    }


    stage('Deploy to Kubernetes (Helm)') {
      steps {
        script {
          echo "🚀 deploying to kubernetes via helm..."
          sh """
            # use the copied kubeconfig file
            export KUBECONFIG=/var/lib/jenkins/kubeconfig
            
            # verify connection
            kubectl cluster-info
            kubectl get nodes
            
            # deploy with helm
            helm upgrade --install ${HELM_RELEASE} ./helm \
              --set image.repository=${IMAGE} \
              --set image.tag=${TAG} \
              --namespace ${NAMESPACE} --create-namespace
          """
        }
      }
    }
  }

  post {
    success {
      echo "✅ Pipeline Sukses: Aplikasi berhasil dideploy ke Kubernetes"
    }
    failure {
      echo "❌ Pipeline Gagal: Cek log untuk mengetahui error"
    }
  }
}
