pipeline {
    agent any
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        //stage('Cache Dependencies') {
           // steps {
             //   script {
                    // Cache des dépendances Maven dans le répertoire .m2/repository
               //     dir('.m2/repository') {
                        // Utilisation du plugin Pipeline Utility Steps pour la mise en cache
                        // Cette étape suppose que le plugin "Pipeline Utility Steps" est installé sur votre instance Jenkins
                        // Si ce n'est pas le cas, vous devrez l'installer depuis le Gestionnaire de plugins de Jenkins
                        // La syntaxe ci-dessous est spécifique au plugin Pipeline Utility Steps
                        // Elle crée une clé de cache basée sur le nom du job et la vérifie avant d'effectuer un téléchargement complet
                        // Si une cache est disponible, elle est extraite, sinon, elle est téléchargée à partir du stockage distant
                        // Pour plus d'options et de configurations, consultez la documentation du plugin Pipeline Utility Steps
                 //       withCache(cacheKey: 'maven-dependencies', cacheValue: 'maven-dependencies', path: '.m2/repository') {
                   //         sh './mvnw dependency:resolve'
                    //    }
                   // }
               // }
           // }
       // }
        stage('Build') {
            steps {
                script {
                    sh 'mvn compile'
                }
            }
        }
	stage('Test') {
	    steps {
	        script {
	            sh 'mvn test -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true --batch-mode --show-version'
	        }
	    }
	    post {
	        always {
	            junit 'target/surefire-reports/*.xml'
	        }
	    }
	}
//	stage('Build') {
        //    steps {
      //          withMaven(maven: 'mvnw') {
                   // script {
                        // Configure Cache-Maven
                    //    def mvnCache = new MavenCache()

                        // Use the cache for the compile step
                  //      mvnCache.useCache {
                 //           sh './mvnw compile -Dmaven.repo.local=$WORKSPACE/.m2/repository -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true --batch-mode --show-version'
               //         }
             //       }
           //     }
         //   }
       // }

       // stage('Test') {
           // steps {
               // withMaven(maven: 'mvnw') {
                //    script {
                        // Configure Cache-Maven
                      //  def mvnCache = new org.jenkinsci.plugins.maven.cache.MavenCache()

                        // Use the cache for the test step
                     //   mvnCache.useCache {
                   //         sh './mvnw test -Dmaven.repo.local=$WORKSPACE/.m2/repository -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true --batch-mode --show-version'
                 //       }
               //     }
             //   }
           // }
	  //  post {
	//	always {
	//	    junit 'target/surefire-reports/*.xml'
	//	}
	  //  }
        //}
	stage('Quality') {
            steps {
                script {
                    docker.image('codeclimate/codeclimate').inside {
                        sh 'mkdir codequality-results'
                        sh 'codeclimate analyze -f html > ./codequality-results/index.html'
                    }
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: 'codequality-results/*'
                }
            }
        }
	stage('Package') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'ivann77','17MenSar321!!') { // Replace with your registry URL and credentials
                        sh './mvnw install -PbuildDocker -DskipTests=true -DpushImage -Dmaven.repo.local=$WORKSPACE/.m2/repository -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true --batch-mode --show-version'
                        sh 'docker compose build'
                        sh 'docker compose push'
                    }
                }
            }
        }

    }
}




