# Simple Python Flask Dockerized Application#

Build the image using the following command

```bash
$ docker build -t captain:latest .
```

Run the Docker container using the command shown below.

```bash
$ docker run -d -p 8083:5000 simple-flask-app
```

The application will be accessible at http:127.0.0.1:5000
