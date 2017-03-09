node {

	stage 'Checkout'
	checkout scm
	stash 'workspace'

	stage 'Unit Test'
	sleep 5
	
	stage 'Deploy on tests platforms'
	sleep 5
	
	stage 'QA Tests & Performance Tests'
	parallel (
		phase1: { sh "echo p1; sleep 10s; echo phase1 - Qa Tests" },
		phase2: { sh "echo p2; sleep 15s; echo phase2 - Perf Tests" }
	)
	
	echo "Tests Ok"   
	
	stage 'Mobile testing'
	lock('Samsung-galaxy-Note-7') {
	echo 'Battery tests :)'
	// any other build will wait until the one locking the resource leaves this block
	}
	
}

input 'Mobile testing OK, nothing exploded ?'

node {
	
	stage 'Integration tests'
	unstash 'workspace'
	sleep 5

}

if (env.BRANCH_NAME == 'master') {

  milestone 1
	input message: 'Release ?', submitter: 'antoine_c'
	milestone 2
  
	node {

		stage 'release'
		unstash 'workspace'	
		sleep 5

	}

  milestone 3
	input message: 'Deploy to prod ?', submitter: 'big_boss'
	milestone 4

	node {

		stage 'Deploy to prod?'	
		unstash 'workspace'
		
		stage 'Deploy to Prod'
		sleep 5
    
    timeout(1) {
    waitUntil {
        def r = sh script: 'wget -q https://jenkins.io/ -O /dev/null', returnStatus: true
        return (r == 0);
    }
    slackSend channel: 'aperotech_pipeline', color: 'good', message: 'Prod OK', teamDomain: 'valeuriad', tokenCredentialId: 'slackbot'
    }

	}
}