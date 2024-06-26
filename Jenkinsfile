node {
   // -- Configura variables
   echo 'Configurando variables'
   def mvnHome = tool 'M3'
   env.PATH = "${mvnHome}/bin:${env.PATH}"
   echo "var mvnHome='${mvnHome}'"
   echo "var env.PATH='${env.PATH}'"

   // -- Descarga código desde SCM
   echo 'Descargando código de SCM'
   sh 'rm -rf *'
   checkout scm
   stage('Package') {
       dir('PruebaDockerSVN') {
            sh 'mvn clean package -DskipTests'
       }
   }

   stage('Create Docker Image') {
       dir('PruebaDockerSVN') {
           docker.build("prueba-docker-svn")
       }
   }

   stage ('Run Application') {
       dir('PruebaDockerSVN') {
           try { 
               sh "docker run -it --rm --volume 
               "$PWD"/pom.xml://usr/src/app/pom.xml 
               \ --volume "$HOME"/.m2:/root/.m2 maven:3-jdk-8-alpine mvn 
               install"


           } catch (error) {
           } finally {}
        }
   }
}
