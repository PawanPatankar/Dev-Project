pipeline {
  agent { label 'build' }

  environment { 
    registry = "adamtravis/democicd" 
    registryCredential = 'dockerhub' // DockerHub credential ID (same as before)
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', 
            credentialsId: 'GithubCred', // 👉 Replace with your GitHub credentials ID in Jenkins
            url: 'https://github.com/your-org-or-username/your-repo-name.git' // 👉 Change this to your GitHub repo
      }
    }

    stage('Stage I: Build') {
      steps {
        echo "Building Jar Component ..."
        sh "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64; mvn clean package"
      }
    }

    stage('Stage II: Code Coverage') {
      steps {
        echo "Running Code Coverage ..."
        sh "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64; mvn jacoco:report"
      }
    }

    stage('Stage III: SCA') {
      steps { 
        echo "Running Software Composition Analysis using OWASP Dependency-Check ..."
        sh "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64; mvn org.owasp:dependency-check-maven:check"
      }
    }

    stage('Stage IV: SAST') {
      steps { 
        echo "Running Static application security testing using SonarQube Scanner ..."
        withSonarQubeEnv('mysonarqube') {
          sh '''
            mvn sonar:sonar \
              -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
              -Dsonar.dependencyCheck.jsonReportPath=target/dependency-check-report.json \
              -Dsonar.dependencyCheck.htmlReportPath=target/dependency-check-report.html \
              -Dsonar.projectName=wezvatech
          '''
        }
      }
    }

    stage('Stage V: Quality Gates') {
      steps { 
        echo "Running Quality Gates to verify the code quality"
        script {
          timeout(time: 1, unit: 'MINUTES') {
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
              error "Pipeline aborted due to quality gate failure: ${qg.status}"
            }
          }
        }
      }
    }

    stage('Stage VI: Build Image') {
      steps { 
        echo "Build Docker Image"
        script {
          docker.withRegistry('', registryCredential) { 
            def myImage = docker.build(registry)
            myImage.push()
          }
        }
      }
    }

    stage('Stage VII: Scan Image') {
      steps { 
        echo "Scanning Image for Vulnerabilities"
        sh "trivy image --scanners vuln --offline-scan ${registry}:latest > trivyresults.txt"
      }
    }

    stage('Stage VIII: Smoke Test') {
      steps { 
        echo "Smoke Test the Image"
        sh "docker run -d --name smokerun -p 8080:8080 ${registry}"
        sh "sleep 90; ./check.sh"
        sh "docker rm --force smokerun"
      }
    }

  }
}
