pipeline {
  agent any

  environment {
    GITHUB_TOKEN = credentials('jenkin token')
    BASE_BRANCH = 'main'
  }

  stages {
    stage('Tạo Pull Request nếu không phải nhánh main') {
      when {
        expression { env.GIT_BRANCH != "origin/${env.BASE_BRANCH}" }
      }
      steps {
        script {
          def branch = sh(script: "git rev-parse --abbrev-ref HEAD", returnStdout: true).trim()

          sh """
            echo \$GITHUB_TOKEN | gh auth login --with-token
            gh pr create --base ${env.BASE_BRANCH} --head ${branch} \
              --title "Auto PR from ${branch}" \
              --body "Pull request được tạo tự động từ Jenkins."
          """
        }
      }
    }
  }
}
