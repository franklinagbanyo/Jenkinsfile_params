pipeline {
    agent any

    parameters {
        string(name: 'USERNAME', defaultValue: '', description: 'Enter username')
        string(name: 'PASSWORD', defaultValue: '', description: 'Enter password')
        string(name: 'EMAIL', defaultValue: '', description: 'Enter email')
        string(name: 'FIRSTNAME', defaultValue: '', description: 'Enter first name')
        string(name: 'LASTNAME', defaultValue: '', description: 'Enter last name')

        choice(
            name: 'ACCOUNT_MANAGEMENT',
            choices: ['', 'add_user', 'delete_user', 'lock_user', 'unlock_user'],
            description: 'Select account management action'
        )
    }

    stages {
        stage('Sanity Check') {
            steps {
                script {
                    sanity_check()
                }
            }
        }
        stage('Collect Information') {
            steps {
                script {
                    collect_information()
                }
            }
        }
        stage('User Action') {
            when {
                expression { params.ACCOUNT_MANAGEMENT != '' }
            }
            steps {
                script {
                    user_action()
                }
            }
        }
        stage('Post-Build Actions') {
            steps {
                script {
                    echo "Build Status: ${currentBuild.result}"

                    try {
                        if (currentBuild.resultIsBetterOrEqualTo('SUCCESS')) {
                            echo 'Build successful! Performing post-success actions...'
                            // Add your post-success actions here
                        }
                    } catch (Exception e) {
                        echo "Error in post-success actions: ${e.message}"
                    }

                    try {
                        if (currentBuild.resultIsWorseOrEqualTo('FAILURE')) {
                            echo 'Build failed! Performing post-failure actions...'
                            // Add your post-failure actions here
                        }
                    } catch (Exception e) {
                        echo "Error in post-failure actions: ${e.message}"
                    }
                }
            }
        }
    }
}

def sanity_check() {
    // Existing sanity check code
}

def collect_information() {
    // Existing collect information code
}

def user_action() {
    def accountAction = params.ACCOUNT_MANAGEMENT

    switch (accountAction) {
        case 'add_user':
            echo 'Performing Add User action...'
            create_user_account()
            break
        case 'delete_user':
            echo 'Performing Delete User action...'
            delete_user_account()
            break
        case 'lock_user':
            echo 'Performing Lock User action...'
            lock_user_account()
            break
        case 'unlock_user':
            echo 'Performing Unlock User action...'
            unlock_user_account()
            break
        default:
            error "Invalid account management action selected: ${accountAction}"
    }
}

def create_user_account() {
    sh """
        sudo useradd -m ${params.USERNAME}
        echo "${params.USERNAME}:${params.PASSWORD}" | sudo chpasswd
        sudo usermod -c "${params.FIRSTNAME} ${params.LASTNAME}" ${params.USERNAME}
        sudo cat /etc/passwd
        sudo cat /etc/shadow
        ls /home
    """
}

def delete_user_account() {
    sh "sudo userdel -r ${params.USERNAME}"
}

def lock_user_account() {
    sh "sudo passwd -l ${params.USERNAME}"
}

def unlock_user_account() {
    sh "sudo passwd -u ${params.USERNAME}"
}
