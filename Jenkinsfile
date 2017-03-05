node {
	if (env.BRANCH_NAME != 'master') {

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
	milestone 1
	
	stage 'Mobile testing'
	lock('Samsung-galaxy-Note-7') {
	echo 'Battery tests :)'
	// any other build will wait until the one locking the resource leaves this block
	}
	
	}
}
input 'Mobile testing OK, nothing exploded ?'

node {
	
	unstash 'workspace'
	
	stage 'Integration tests'
	sleep 5
	
	stage 'Deploy to prod?'	
}

input message: 'Deploy to prod ?', submitter: 'big_boss'
milestone 2

node {

	unstash 'workspace'
	
	stage 'Deploy to Prod'
	sleep 5

}