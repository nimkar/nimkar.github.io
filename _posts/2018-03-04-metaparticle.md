---
layout: post
title: "Metaparticle: Infrastructure IN the Code !"
excerpt: Metaparticle allows you to define your infrastructure INSIDE your code. Currently in beta, it is created by one of the creators of Kubernetes.
date: 2018-03-04 15:30:00
categories:
  - kubernetes
  - docker
  - metaparticle
  - DevOps
  - Infrastructure
tag:
  - kubernetes
  - docker
  - metaparticle
  - DevOps
  - Infrastructure
---
![Different Containers, Different Applications](/images/business-1853439_640.jpg)
# Metaparticle: Infrastructure IN the Code !

After your code is written, it needs to be packaged and deployed. You can use any number of solutions for orchestrations. You have Terraform, CloudFormation, Vagrant, etc. However for each orchestration solution you have to maintain two different artifacts. 

1. The code, binaries and other artifacts. 
2. The instructions for packaging and deployment. Such as creating, deploying and scaling the containers. 

Each of these artifacts are usually managed independent of each other. 

Metaparticle offers a unique solution in the sense the logic to containerize, deploy and scale your application can be contained within your code itself. This make your application ready to deploy on docker via kubernetes right from the start.
E.g. if you had a small python webserver then all it would take to pack it into a container is to add a @containerize() decorator. Consider the example below.

```python 
from six.moves import SimpleHTTPServer, socketserver
import socket
from metaparticle import containerize

OK = 200

port = 8080

class MyHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(OK)
        self.send_header("Content-type", "text/plain")
        self.end_headers()
        self.wfile.write("Hello Metparticle [{}] @ {}\n".format(self.path, socket.gethostname()).encode('UTF-8'))
        print("request for {}".format(self.path))
    def do_HEAD(self):
        self.send_response(OK)
        self.send_header("Content-type", "text/plain")
        self.end_headers()

@containerize(
    'docker.io/your-docker-user-goes-here', options={'name': 'my-image', 'publish': True})
def main():
    Handler = MyHandler
    httpd = socketserver.TCPServer(("", port), Handler)
    httpd.serve_forever()

if __name__ == '__main__':
    main()
```    

Not only can you deploy on kubernetes easily but you can also leverage Metaparticle to implement various methodologies easily like master election and locking. 

Metaparticle is still in beta. In my tests on Macbook using Minikube and kubectl, I suffered many crashes and the system in general was very unstable. Python 3.5 or greater is also a requirement if you wish to use the Python extensions. 

You can find more examples and tutorials [here](https://metaparticle.io/tutorials/python/) 