resource "aws_vpc" "Vcube138" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "Amar-vpc"
  }
}
##public subnet
resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.Vcube138.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "ap-southeast-1a"
  tags = {
    Name = "public"
  }
}
##private subnet
resource "aws_subnet" "private" {
  vpc_id                  = aws_vpc.Vcube138.id
  cidr_block              = "10.0.2.0/24"
  availability_zone       = "ap-southeast-1b"
  tags = {
    Name = "private"
  }
}
##internet gateway
resource "aws_internet_gateway" "amar_igw" {
  vpc_id = aws_vpc.Vcube138.id
  tags = {
    Name = "amar-igw"
  }
}
##public route table
resource "aws_route_table" "public_route" {
  vpc_id = aws_vpc.Vcube138.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.amar_igw.id
  }

  tags = {
    Name = "public-route"
  }
}
##private route table
resource "aws_route_table" "private_route" {
  vpc_id = aws_vpc.Vcube138.id
  tags = {
    Name = "private-route"
  }
}
##public route table association
resource "aws_route_table_association" "a" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public_route.id
}
##private route table association
resource "aws_route_table_association" "b" {
  subnet_id      = aws_subnet.private.id
  route_table_id = aws_route_table.private_route.id
}
