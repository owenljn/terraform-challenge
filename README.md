# Terraform-challenge
For Ansys Senior DevOps Role

## Requirements

- **Terraform**: Assume it's already installed.
- **AWS CLI**: Assume it's already configured with valid credentials and a default region.
- **AWS Account**: Assume the access to an AWS account to provision resources is available.

---

## Deployment Instructions

**terraform init**

**terraform validate**

**terraform plan**

**terraform apply**

## To verify the port 80 is open

Update the ec2 instance with the user data as shown below so there's a service listening to port 80:

resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public[0].id
  security_groups = [aws_security_group.web.id]

  user_data = <<-EOF
    #!/bin/bash
    sudo yum update -y
    sudo yum install -y python3
    echo "Hello, World!" > /home/ec2-user/index.html
    nohup python3 -m http.server 80 --directory /home/ec2-user/ > /home/ec2-user/http-server.log 2>&1 &
  EOF

  tags = {
    Name = "web-instance"
  }
}

**curl http://<ec2_instance_public_ip>**

Or simply run **nmap -p 80 <ec2_instance_public_ip>** command to see if port 80 is reachable (note it will say port 80 is closed because no service is running in the instance)

## To clean up

**terraform destroy**
