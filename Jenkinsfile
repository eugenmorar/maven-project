node('master') {

  stage('Checkout') {
    git 'https://github.com/eugenmorar/maven-project.git'
  }

  stage('Sonar Analysis') {
    withSonarQubeEnv('LocalSonarServer') {
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    } 
  }

  stage('Build & Archive') {
   sh 'mvn clean package'      
   echo 'Archiving ...'
   archiveArtifacts artifacts: '**/target/webapp.war'   	
  }
  
  stage('Deploy on Tomcat') {
   sh 'scp -r **/target/webapp.war root@192.168.109.102:/var/lib/tomcat8/webapps/webapp.war'	
  }

}
