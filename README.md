# -create-vpc_using-terraform

resource "aws_vpc" "Vcube138" {
  cidr_block       = "11.0.0.0/16"
  tags =   {
     Name = "Amar-vpc"
  }
}
resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.Vcube138.id
  cidr_block = "11.0.1.0/24"

  tags = {
    Name = "public"
  }
}
resource "aws_subnet" "pravite" {
  vpc_id     = aws_vpc.Vcube138.id
  cidr_block = "11.0.2.0/24"

  tags = {
    Name = "private"
  }
}
resource "aws_route_table" "public-route" {
  vpc_id = aws_vpc.Vcube138.id

  route {
    cidr_block = "11.0.1.0/24"
    gateway_id = aws_internet_gateway.amar-igw.id
  }

  route {
    ipv6_cidr_block        = "::/0"
    egress_only_gateway_id = aws_egress_only_internet_gateway.example.id
  }

  tags = {
    Name = "public-route"
  }
}
resource "aws_route_table" "private-route" {
  vpc_id = aws_vpc.Vcube138.id

  route {
    cidr_block = "11.0.1.0/24"
  }

  route {
    ipv6_cidr_block        = "::/0"
    egress_only_gateway_id = aws_egress_only_internet_gateway.example.id
  }

  tags = {
    Name = "private-route"
  }
}
resource "aws_internet_gateway" "amar-igw" {
  vpc_id = aws_vpc.Vcube138.id

  tags = {
    Name = "amar-igw"
  }
}
resource "aws_route_table_association" "a" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public-route.id
}
resource "aws_internet_gateway_attachment" "example" {
  internet_gateway_id = aws_internet_gateway.example.id
  vpc_id              = aws_vpc.example.id
}

resource "aws_vpc" "Vcube138" {
  cidr_block = "11.0.0.0/16"
}

resource "aws_internet_gateway" "amar-igw" {}
