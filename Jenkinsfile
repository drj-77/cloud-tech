pipeline {
    agent any
    environment = {
        AWS_ACCESS_KEY_ID = "AKIASTUXGMLMJTB5MRMT"
        AWS_SECRET_ACCESS_KEY = "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"
        AWS_DEFAULT_REGION = "ap-south-1"
        PEM_FILE= "Ec2-key-21-june24.pem"

     stages {
        stage('Launch EC2 Instance') {
            steps {
                script {
                    def instanceType = 't2.micro' // Change this to your desired instance type
                    def amiId = 'ami-0f58b397bc5c1f2e8' // Change this to your desired AMI ID
                    def subnetId = 'subnet-0a9a4738f46e6f7aa' // Change this to your desired subnet ID
                    def securityGroupId = 'sg-09ade844d7b1ed258' // Change this to your desired security group ID
                    
                    def ec2 = [
                        imageId: amiId,
                        type: instanceType,
                        securityGroup: securityGroupId,
                        subnetId: subnetId,
                        region: AWS_DEFAULT_REGION,
                        awsAccessKeyId: AWS_ACCESS_KEY_ID,
                        awsSecretAccessKey: AWS_SECRET_ACCESS_KEY
                    ]
                    
                    def launchResult = awsEC2LaunchInstance(ec2)
                    echo "Launched instance: ${launchResult.instanceId}"
                }
            }
        }
    }
}  
}

// Function to launch EC2 instance
def awsEC2LaunchInstance(ec2Config) {
    def AmazonEC2 = new com.amazonaws.services.ec2.AmazonEC2Client()
    AmazonEC2.setRegion(com.amazonaws.regions.RegionUtils.getRegion(ec2Config.region))
    
    def runInstancesRequest = new com.amazonaws.services.ec2.model.RunInstancesRequest()
        .withImageId(ec2Config.imageId)
        .withInstanceType(ec2Config.type)
        .withSecurityGroupIds(ec2Config.securityGroup)
        .withSubnetId(ec2Config.subnetId)
    
    def runInstancesResult = AmazonEC2.runInstances(runInstancesRequest)
    
    return runInstancesResult.getReservation().getInstances().get(0)
}
