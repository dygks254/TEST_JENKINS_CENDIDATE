

def configuration() {
    slurmList                    = "T1 T2 T3 T4 T5 T6"
    semaphoreSize                = 10
}
def do_configuration = configuration()
def branches = [:]


pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                print("Build")
            }
        }
        stage('Test') {
            steps {
                script{
                    def semaphore = createSemaphore(permit : semaphoreSize)
                    branches = slurmList.split(' ');
                    def buildJob = [:]
                    branches.each { f ->
                        buildJob["${f}"] = {
                            acquireSemaphore(semaphore){
                                stage("${f}"){
                                    print("")
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
