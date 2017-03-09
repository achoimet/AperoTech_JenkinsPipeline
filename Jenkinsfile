node {
  checkout scm
  
	stage 'Build'
  sh 'npm install --global mocha'
	sh 'npm install'
  
	stash 'workspace'

	stage 'Unit Test'
	sh 'mocha tests --recursive'
	
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

	input message: 'Release ?', submitter: 'antoine_c'
	milestone 1
	node {

		stage 'release'
		unstash 'workspace'	
		sleep 5

	}

	input message: 'Deploy to prod ?', submitter: 'big_boss'
	milestone 2

	node {

		stage 'Deploy to prod?'	
		unstash 'workspace'
		
		stage 'Deploy to Prod'
		sleep 5

	}
}