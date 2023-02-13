

def configuration() {
    slurmList                    = "T1 T2 T3 T4 T5 T6"
    semaphoreSize                = 10
}
def do_configuration = configuration()
def branches = [:]

def defaultTestlist = ""

pipeline {
    agent any
    parameters {
       string(name: 'hostname', defaultValue: 'gabor-dev', description: 'Hostname or IP address')
       choice(name: 'planet', choices: ['Mercury', 'Venus', 'Earth', 'Mars'], description:  'Pick a planet')
       choice(name: 'space', choices: ['', 'Mercury', 'Venus', 'Earth', 'Mars'], description:  'Pick a planet. Defaults to empty string')
       text(name: 'story', defaultValue: 'One', description: '')
       password(name: 'secret', defaultValue: '', description: 'Type some secret')

    }
    stages {
        stage('Build') {
            steps {
                print("Build")
            }
        }
        stage('Default_value'){
            steps{
                echo "--------"
                // echo "${params.hostname}"
                // echo "${params.planet}"
                // echo "${params.space}"
                // echo "${params.story}"
                // echo "${params.secret}"
                script {
                    sh """
                        echo ${params.hostname}
                        echo ${params.planet}
                        echo ${params.space}
                        echo ${params.story}
                        echo ${params.secret}
                    """
                }
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
