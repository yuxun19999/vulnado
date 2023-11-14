pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/yuxun19999/vulnado.git'
            }
        }

        stage('Build') {
            steps {
                sh '/var/jenkins_home/apache-maven-3.6.3/bin/mvn --batch-mode -V -U -e clean verify -Dsurefire.useFile=false -Dmaven.test.failure.ignore'
            }
        }

        stage('Analysis') {
            steps {
                sh '/var/jenkins_home/apache-maven-3.6.3/bin/mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs'
            }
        }
    }

    post {
        always {
            junit testResults: '**/target/surefire-reports/TEST-*.xml'
        }

        success {
            recordIssues enabledForFailure: true, tools: [
                mavenConsole(),
                java(),
                javaDoc(),
                checkStyle(),
                spotBugs(pattern: '**/target/findbugsXml.xml'),
                cpd(pattern: '**/target/cpd.xml'),
                pmdParser(pattern: '**/target/pmd.xml')
            ]
        }
    }
}
