pipeline {
  agent any

  environment {
    IMAGE = "john1436gilbert/devops-demo"
    GITOPS_REPO = "https://github.com/john1436/devops-demo-gitops.git"
  }

  stages {

    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Push') {
      steps {
        sh """
          docker build -t $IMAGE:${BUILD_NUMBER} .
          docker push $IMAGE:${BUILD_NUMBER}
        """
      }
    }

    stage('Update GitOps') {
      steps {
        sh """
          rm -rf gitops
          git clone $GITOPS_REPO gitops
          cd gitops/devops-demo
          sed -i 's/tag:.*/tag: "${BUILD_NUMBER}"/' values.yaml
          git add values.yaml
          git commit -m "Update image to ${BUILD_NUMBER}"
          git push
        """
      }
    }
  }
}

