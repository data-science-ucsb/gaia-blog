+++
title = "Deploying a full-stack web application"
date = 2021-04-03
description = "🌳"
+++


Between lines of code and a website lies the world of
[DevOps](https://en.wikipedia.org/wiki/DevOps): "Development Operations."
Arguably, DevOps is the most overlooked—and challenging—parts of the software
development lifecycle. Nevertheless, it is still an equally crucial process, and
it involves everything from version control to build to deployment and scaling.

The DevOps cycle is not separate from the code: It is a _part_ of the code.
For jadeocr, we used Git for our [version control](https://en.wikipedia.org/wiki/Version_control)
system and [GitHub](https://github.com/jadeocr) as our remote repository and
project management hub. The platform is where we keep track of issues and manage
collaborating on code. We also use GitHub as part of our
[CI/CD pipeline](https://www.redhat.com/en/topics/devops/what-is-ci-cd)
to automatically build the website and server and make the changes live on the
website.

Pushing code to the master branch of our GitHub repository triggers numerous
GitHub workflows that comprise our CI/CD process. GitHub first sends a ping
across the web—an [HTTP POST](https://www.codecademy.com/articles/http-requests)
request—to our server. After receiving the request from GitHub, a continuous deployment
server written in Node.js—previously in Golang—quickly goes to work, pulling
the latest code from the GitHub master branch and uses
[Docker Compose](https://docs.docker.com/compose/) to build and start the
application [containers](https://www.docker.com/resources/what-container),
which are isolated environments called in which application code can run. The
server simply executes a series of shell commands after it receives a request
at a particular [API](https://en.wikipedia.org/wiki/API) endpoint; from a
technical perspective, therefore, this system is quite simple.

Afterward, a GitHub action builds the jadeocr-next client and the server into
their respective Docker images and publishes them to the
[GitHub Container Registry](https://docs.github.com/en/packages/guides/about-github-container-registry).
These containers solve the common issue of "it works on my machine" and make
building, deploying, and scaling applications with
[Kubernetes](https://en.wikipedia.org/wiki/Kubernetes) relatively straightforward
[^1]. Although I faced several (frustrating) hurdles while writing the
initial configuration files for these Docker containers, managing them after
the initial setup process has been a breeze.

Perhaps the most complex individual cog in this software
development machine is [Nginx](https://en.wikipedia.org/wiki/Nginx) (Engine-X)
server and reverse proxy. Nginx is simply a web server, but it can also function
as a [reverse proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)
to direct web traffic to its intended destination. We use Nginx in two places:
in front of the website's static files and in front of the entire website.

When we build the website files into the HTML, CSS, and JavaScript that the
browser renders after loading [jadeocr-next](https://next.jadeocr.com), our web
framework's build tool, [Vue CLI](https://cli.vuejs.org/), creates a directory
of static files that must be served by a web server. Therefore, we use Nginx in
front of the client to serve these static files. We then use _another_ Nginx
server in our cloud virtual machine to direct API requests to our web server
and website requests to the aforementioned client Nginx server. 

Although each system is fairly simple on its own, the confluence of all these
tools in a single pipeline creates many moving parts which are challenging—but
fun—to manage. Perhaps such a DevOps system is overkill for such a small
application, but it provides jadeocr-next with the ability to easily scale and
avoid future errors. Most importantly, however, going through this DevOps
journey has been a valuable and fulfilling experience for me—even if I do
sometimes spend multiple days staring at a terminal window in confusion.

`:wq`

---

[^1]: For now, we are running jadeocr-next on a virtual private server (VPS),
  but we do plan to deploy a Kubernetes cluster in the future.

