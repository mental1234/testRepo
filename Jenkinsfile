def bucket = 'testbucket-deploy-lambda'
def functionName = 'HelloWorldTest'
def region = 'us-east-2'

node('master'){
	stage('Checkout'){
		checkout scm
	}
	stage('BuildPackage'){
		sh "zip ${commitID()}.zip helloWorld.js"
	}
	stage('Push'){
		sh "aws s3 cp ${commitID()}.zip s3://${bucket}"
	}
	stage('Deploy'){
		sh "aws lambda update-function-code --function-name ${functionName} \
			--s3-bucket ${bucket} \
			--s3-key ${commitID()}.zip \
			--region ${region}
	}
}

def commitID() {
	sh 'git rev-parse HEAD > .git/commitID'
	def commitID = readFile(.git/commitID).trim()
	sh 'rrm .git/commitID'
	commitID
}
