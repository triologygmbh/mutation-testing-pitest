#!groovy
node {
    SONAR_URL = "sonar.qube"
    USERNAME = "username"
    PASSWORD = "password"

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        mvn "clean install -DskipTests"
    }

    stage('Test') {
        String jacoco = "org.jacoco:jacoco-maven-plugin:0.7.9"
        mvn "${jacoco}:prepare-agent test ${jacoco}:report"
    }

    stage('Mutation Test') {
        mvn "org.pitest:pitest-maven:mutationCoverage"
    }

    stage('SonarQube') {
        withCredentials([credentials]) {
            //noinspection GroovyAssignabilityCheck
            mvn "org.codehaus.mojo:sonar-maven-plugin:3.2:sonar -Dsonar.host.url=${SONAR_URL} " +
                    "-Dsonar.login=${USERNAME} -Dsonar.password=${PASSWORD} -Dsonar.exclusions=target/** " +
                    "-Dsonar.pitest.mode=reuseReport"
        }
    }
}

void mvn(String args) {
    sh "./mvnw --batch-mode -V -U -e ${args}"
}
