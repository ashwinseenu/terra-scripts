terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["al2023-ami-2023.*-x86_64"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

resource "aws_instance" "compute_block" {
  ami = data.aws_ami.latest_amazon_linux.id
  instance_type     = "t3.micro"
  key_name          = "pemkey"
  count             = 2
  availability_zone = "us-east-1f"

  tags = {
    Name = "web-${count.index}"
  }
}

resource "aws_s3_bucket" "data_block" {
  count  = 2
  bucket = "user53-s3-${count.index}"

  tags = {
    Name        = "s3"
    Environment = "Dev"
  }
}

