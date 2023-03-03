pipeline {
    agent { 
        node {
            label 'docker-tdp-builder'
            }
      }
    triggers {
        pollSCM '0 1 * * *'
      }
    stages {
        stage('clone') {
            steps {
                echo "Cloning..."
                git branch: 'hbase-spark-2.3.5-1.0.0-TDP-0.1.0-SNAPSHOT-alliage', url: 'https://github.com/Yanis77240/hbase-connectors'
            }
        }
        stage('Build') {
            steps {
                echo "Building..."
                sh '''
                mvn clean install -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT -Dscala.version=2.11.8 -Dscala.binary.version=2.11 -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT -DskipTests -Dmaven.javdoc.skip=true -Dcheckstyle.skip=true -Dfindbugs.skip=true -Dspotbugs.skip=true --batch-mode -fae
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing..."
                sh '''
                mvn clean test -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT -Dscala.version=2.11.8 -Dscala.binary.version=2.11 -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT -Dmaven.javdoc.skip=true -Dcheckstyle.skip=true -Dfindbugs.skip=true -Dspotbugs.skip=true --batch-mode -fae --fail-never
                '''
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                echo "Deploy..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'mvn clean deploy -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT -Dscala.version=2.11.8 -Dscala.binary.version=2.11 -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT -DskipTests -Dmaven.javdoc.skip=true -Dcheckstyle.skip=true -Dfindbugs.skip=true -Dspotbugs.skip=true --batch-mode -fae -s settings.xml'
                }
            }        
        }
    }
}