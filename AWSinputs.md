# Inputs for Completing Tasks

## Task 1: Deploy a Simple Web Page on an EC2 Instance

### User Data Script for Apache Web Server
```bash
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo "<html><h1>Hello, World!</h1></html>" > /var/www/html/index.html
```

### Security Group Rules
- **Inbound Rules:**
  - SSH (port 22): Allow traffic from your IP address
  - HTTP (port 80): Allow traffic from anywhere (`0.0.0.0/0`)

---

## Task 2: Deploy a Flask Application on an EC2 Instance

### Commands to Install Python 3 and Flask
```bash
sudo yum update -y
sudo yum install -y python3
python3 -m pip install flask
```

### Flask App (`app.py`)
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Your name"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

---

## Task 3: Add Routes to the Flask Application

### Updated `app.py` with Routes
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Welcome to the Homepage!"

@app.route('/about')
def about():
    return "This is a simple Flask application."

@app.route('/contact')
def contact():
    return "Contact us at: contact@example.com"

@app.route('/status')
def status():
    return "The app is running perfectly!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

---

## Task 4: Set Up a Static Website Using Amazon S3

### Sample HTML File (`index.html`)
```html
<!DOCTYPE html>
<html>
<head>
    <title>Portfolio</title>
</head>
<body>
    <h1>Welcome to My Portfolio!</h1>
    <p>This is a simple static website hosted on Amazon S3.</p>
</body>
</html>
```

### Example Bucket Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-portfolio-site/*"
        }
    ]
}
```

---

## Task 5: Deploy a Dockerized Web Application on EC2 Instances

### User Data Script for Docker Installation
```bash
#!/bin/bash
sudo yum update -y
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
```

### Docker Run Command
```bash
docker run -d -p 80:80 nginx
```

---

## Task 6: Create and Use a Custom AMI with Docker Pre-installed

### Verify Docker Installation
```bash
sudo docker --version
sudo systemctl status docker
```

---

## Task 7: Set Up a Load Balancer for Dockerized Applications

### ALB Target Group Health Check Path
```
/
```

---

## Task 8: Deploy a Dockerized Node.js Application Using Amazon ECR and ECS

### Sample `app.js` (Node.js Application)
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Welcome to the Travel Memory App!');
});

app.listen(8080, () => {
    console.log('Server running on port 8080');
});
```

### Dockerfile
```dockerfile
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 8080
CMD [ "node", "app.js" ]
```

### Commands to Push Docker Image to ECR
```bash
eval $(aws ecr get-login-password --region <region>) | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
docker build -t travel-memory-app .
docker tag travel-memory-app:latest <account-id>.dkr.ecr.<region>.amazonaws.com/travel-memory-app:latest
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/travel-memory-app:latest
```
