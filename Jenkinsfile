pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git 'https://github.com/lowyl/JenkinsDependencyCheckTest'
                git branch:'master', url: 'https://github.com/ScaleSec/vulnado.git'
			}

		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
                sh '/var/jenkins_home/apache-maven-3.6.3/bin/mvn --batch-mode -V -U -e clean verify -Dsurefire.useFile=false -Dmaven.test.failure.ignore'
			}
                
		}

        stage ('Analysis') {
            steps {
                sh '/var/jenkins_home/apache-maven-3.6.3/bin/mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs'
            }
        }          
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
        always {
            junit testResults: '**/target/surefire-reports/TEST-*.xml'
            recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs(pattern:'**/target/findbugsXml.xml')
            recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
            recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
        }
	}
}
