stage ('Stage 1. Allocate workspace')
def extWorkspace = exwsAllocate 'diskpool1'

node ('linux') {
    exws (extWorkspace) {
        stage('Stage 2. Build on the build server')

        git url:https://github.com/gudipatilakshmi/

        def mvnHome = tool 'M3'
        sh '${mvnHome}/bin/mvn clean install -DskipTests'
    }
}

node ('test') {
    exws (extWorkspace) {
        stage('Stage 3. Run tests on a test machine')

        def mvnHome = tool 'M3'
        sh '${mvnHome}/bin/mvn test'
    }
}
