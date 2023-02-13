
def configuration = [
    'agent'        : "ariel",
    'project_repo' : "",
    'summary_dir'  : ""
]
def branches = [:]

pipeline {
    agent {
        label configuration['agent']
    }
    parameters {
        string(name : 'basic_module', defaultValue: 'sifive/wit/0.14.1', description: 'Defaullt module list')
        string(name : 'summary_dir', defaultValue: '/user/jenkins/summary/tmp_new_summary', description: 'Summary PATH')
        string(name : 'project', defaultValue: 'RHEA_EVT1', description: 'Project name')
        string(name : 'top_repo', defaultValue: 'git@portal:platform/walnutcake/soc-rhea-semifive.git', description: 'Top repository for wit init')
        string(name : 'test_list', defaultValue: 'full_regr_test_list.json', description: 'Run test json or each test')
        string(name : 'proc_semaphore', defaultValue: '10', description: 'Number of parallel running tests')
    }
    stages {
        stage('Setting'){
            steps{
                script{
                    configuration['project_repo'] = params.top_repo.split('/')[-1].replace(".git","")
                    configuration['summary_dir']  = "${params.summary_dir}/${project}"
                    for  (each in configuration){
                        print("This configuration value ::  ${each.key}\t->  ${each.value}")
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script{
                    cleanWs()
                    sh """
                        module load ${basic_module}
                        wit init . -a ${params.top_repo}
                    """
                }
            }
        }
    }
}
