# pulumi-gcp-basics

Create project
```
pulumi new yaml -n helloworld -d "Sample application" -s dev
```

create main.py file
```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(host='0.0.0.0')
``` 

Create Dockerfile
``` 
FROM python
WORKDIR /opt
RUN pip install flask
ADD main.py /opt
CMD python main.py
``` 

Create test.yaml
``` 
name: helloworld
runtime: yaml
description: Hello World
configuration:
  publishingPort:
    type: Number
variables:
  internalPort: 5000
resources:
  backend-image:
    type: docker:index:Container
    properties:
      image: dmitriizolotov/helloworld 
      name: helloworld
      ports:
      - internal: ${internalPort}
        external: ${publishingPort}
```  

launch in CLI
```  
docker build -t dmitriizolotov/helloworld .
docker push dmitriizolotov/helloworld
pulumi up -y
pulumi config set publishingPort 8080
```  

Check localhost 
```  
http://localhost:8080
```
[source](https://habr.com/ru/company/otus/blog/674818/?ysclid=l5upca45eo436475513)
