#launch a instance and insatll appache server on it using user data


##Resource : ec2
resource "aws_instance" "demo" {
  ami           = "ami-076e3a557efe1aa9c"
  instance_type = "t2.micro"
  #to attach user data script to the instace we need file function , file("${path.module}/app1-userdata.sh")  this is the file function 
  user_data     = file("${path.module}/app1-userdata.sh") 
  key_name = "newlinux"
  tags = {
    "Name" = "udemydemo"
  }

}

...............................................................................................................

#app1-userdata.sh that is user data script for installing appache server while launching the instance 

#! /bin/bash
# Instance Identity Metadata Reference - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-identity-documents.html
sudo yum update -y
sudo yum install -y httpd
sudo systemctl enable httpd
sudo service httpd start  
sudo echo '<h1>Welcome to StackSimplify - APP-1</h1>' | sudo tee /var/www/html/index.html
sudo mkdir /var/www/html/app1
sudo echo '<!DOCTYPE html> <html> <body style="background-color:rgb(250, 210, 210);"> <h1>Welcome to Stack Simplify - APP-1</h1> <p>Terraform Demo</p> <p>Application Version: V1</p> </body></html>' | sudo tee /var/www/html/app1/index.html
sudo curl http://169.254.169.254/latest/dynamic/instance-identity/document -o /var/www/html/app1/metadata.html
.....
