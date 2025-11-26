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

data "aws_ami" "latest_ubuntu" {
  most_recent = true
    owners      = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

resource "aws_instance" "compute_block" {
  ami = data.aws_ami.latest_ubuntu.id
  instance_type     = "t2.micro"
  key_name          = "newpem"
  count             = 2
  availability_zone = "us-east-1f"

  tags = {
    Name = "webi-${count.index}"
  }

  lifecycle {
        create_before_destroy=true
  }
}

resource "aws_s3_bucket" "data_block" {
  count  = 2
  bucket = "ash4-s3-${count.index}"

  tags = {
    Name        = "s3"
    Environment = "Dev"
  }
}

