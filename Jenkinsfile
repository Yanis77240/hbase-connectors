podTemplate(containers: [
    containerTemplate(
        name: 'tdp-builder', 
        image: 'yanisbariteau/tdp-builder:jenkins', 
        command: 'sleep', 
        args: '30d'
        )
  ]) {

    node(POD_LABEL) {
        container('tdp-builder') {
            environment {
                number="${currentBuild.number}"
            }
            stage('Git Clone') {
                echo "Cloning.."
                git branch: 'hbase-spark-2.3.5-1.0.0-TDP-0.1.0-SNAPSHOT-alliage-k8s', url: 'https://github.com/Yanis77240/hbase-connectors'
            }   
            stage ('Build') {
                echo "Building.."
                sh '''
                mvn clean install -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT -Dscala.version=2.11.8 -Dscala.binary.version=2.11 -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT -DskipTests -Dmaven.javdoc.skip=true -Dcheckstyle.skip=true -Dfindbugs.skip=true -Dspotbugs.skip=true --batch-mode -fae
                '''
            }
            /*stage('Test') {
                echo "Testing.."
                sh '''
                mvn test -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT -Dscala.version=2.11.8 -Dscala.binary.version=2.11 -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT -Dmaven.javdoc.skip=true -Dcheckstyle.skip=true -Dfindbugs.skip=true -Dspotbugs.skip=true --batch-mode -fae --fail-never
                '''
            }*/
            stage('Deliver') {
                echo "Deploy..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'mvn clean deploy -Dspark.version=2.3.5-TDP-0.1.0-SNAPSHOT -Dscala.version=2.11.8 -Dscala.binary.version=2.11 -Dhadoop-three.version=3.1.1-TDP-0.1.0-SNAPSHOT -Dhbase.version=2.1.10-TDP-0.1.0-SNAPSHOT -DskipTests -Dmaven.javdoc.skip=true -Dcheckstyle.skip=true -Dfindbugs.skip=true -Dspotbugs.skip=true --batch-mode -fae -s settings.xml'
                }
            }
            stage("Publish tar.gz to Nexus") {
                echo "Publish tar.gz..."
                withEnv(["number=${currentBuild.number}"]) {
                    withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh 'curl -v -u $user:$pass --upload-file hbase-connectors-assembly/target/hbase-connectors-1.0.0-TDP-0.1.0-SNAPSHOT-bin.tar.gz http://10.110.4.212:8081/repository/maven-tar-files/hbase-connectors/hbase-connectors-1.0.0-TDP-0.1.0-SNAPSHOT-bin-${number}.tar.gz'
                    }
                }
            }       
        }
    }
}