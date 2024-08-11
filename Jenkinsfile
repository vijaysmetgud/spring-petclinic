pipeline{

agent { label 'jdk-17' }

options{
    timeout(time:30, unit: 'MINUTES')
}

triggers{
    pollSCM('* * * * *')
}

tools{
    jdk 'jdk-17'
}

stages{

    stage('git checkout'){
        steps{
            sh 'echo This is git checkout stage'
        }
    }

    stage('build and deploy'){
        steps{
            sh 'echo This is second stage'
        }
    }

    stage('reporting'){
        steps{
            sh 'echo This id reporting stage'
        }
    }
}
}
