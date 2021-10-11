pipeline {

    // run on jenkins nodes tha has slave label .....

    agent { label 'maven' }

    // global env variables

   /* environment {

        EMAIL_RECIPIENTS = 'akshay.kg@bt.com'

    }*/
    
    stages {
        
        stage('Build') {
            steps {
                // Run the maven build
                sh 'mvn clean install'
                
            }
        }
        stage('Unit Testing junit')
        {
            steps
            {
                       junit 'target/surefire-reports/*.xml'

            }
        }
        stage('Code Coverage')
        {
          steps
          {
         
           jacoco()
           }
        }
        
        stage('Code Quality Check (Sonarqube)')
        {
            environment { 
                projectKey = 'Javawebapp'
                projectName = 'Javawebapp'
                projectVersion = '1.1'
                sonarSources = 'src'
                //sonarExclusions = ''
                sonarLanguage = 'java'
                sonarBinaries = 'target/classes'
                sonarCoverageformat = '-Dsonar.coverage.jacoco.xmlReportPaths'
                coverageReportsPath = 'target/jacoco.xml'
                sonarSourceEncoding = 'UTF-8'
               // coverageExclusions = ''
            }
          steps
          {
             script
             {
               def sonarscanner = tool 'sonar_scanner'
               withSonarQubeEnv('sonarqube') {
               
                    // some block
                    sh """
                    ${sonarscanner}/bin/sonar-scanner -Dsonar.projectKey=${projectKey} \
                        -Dsonar.projectName=${projectName} \
                        -Dsonar.projectVersion=${projectVersion} \
                        -Dsonar.sources=${sonarSources} \
                       # -Dsonar.exclusions=${sonarExclusions} 
                        -Dsonar.language=${sonarLanguage} \
                        -Dsonar.java.binaries=${sonarBinaries} \
                        ${sonarCoverageformat}=${coverageReportsPath} \
                        -Dsonar.c.file.suffixes=- \
                        -Dsonar.cpp.file.suffixes=- \
                        -Dsonar.objc.file.suffixes=- \
                        -Dsonar.sourceEncoding=${sonarSourceEncoding} 
                      #  -Dsonar.sonar.coverage.exclusions=${coverageExclusions} 
                
                    """
                }
             }
          }
        }
        
       /* stage('Quality gate') {

            steps {

                timeout(time: 1, unit: 'MINUTES') {

                    retry(3) {

                        script {

                            def qg = waitForQualityGate()

                            if (qg.status != 'OK') {

                                error "Pipeline aborted due to quality gate failure: ${qg.status}"

                            }

                        }

                    }

                }

            }

        }*/

      /*  stage('Upload to Nexus') {
            steps {
                // Deploy to Nexus
               nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'EED_Engg-Excellence-Devops-POC_maven_releases', packages: []
            }
        }*/
    }
       /* post('Send Email') {
        failure {
            script {
                mail (to: 'wasim.3.akram@bt.com',
                        subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) failed",
                        body: "Please visit ${env.BUILD_URL} for further information"
                );
                }
            }
         success {
             script {
                mail (to: 'wasim.3.akram@bt.com', 
                        subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) success.",
                        body: "Please visit ${env.BUILD_URL} for further information.",


                  );
                }
            }      
         }*/
    }
