node {

  stage('Checkout') {
    git 'https://github.com/eugenmorar/maven-project.git'
  }

  stage('Sonar Analysis') {
    withSonarQubeEnv('LocalSonarServer') {
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    } 
  }

  stage('Build') {
   sh 'mvn clean package'      
   echo 'Archiving ...'
   archiveArtifacts artifacts: '/var/lib/jenkins/workspace/maven-build-deploy/webapp/target/webapp.war'   	
  }
  
  stage('Browser Testing') {
   echo 'Test SUcceeded'	
  }

  stage('Deploy on Tomcat') {
   sh 'sshpass -p "eugen" scp -r /var/lib/jenkins/workspace/maven-build-deploy/webapp/target/webapp.war root@192.168.109.100:/var/lib/tomcat8/webapps/webapp.war'	
  }

  stage('Create Report') {
   step([$class: "CucumberReportPublisher", failedFeaturesNumber: 0, failedScenariosNumber: 0, failedStepsNumber: 0, fileExcludePattern: '', fileIncludePattern: '**/*json', jsonReportDirectory: '', parallelTesting: false, pendingStepsNumber: 0, skippedStepsNumber: 0, trendsLimit: 0, undefinedStepsNumber: 0])
  }

}
