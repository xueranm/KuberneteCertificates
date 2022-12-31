# KCNA Preparation 
## Pre-Knowledge/Skills requirements


<img width="479" alt="image" src="https://user-images.githubusercontent.com/24993672/210115793-3dfc5f75-37d3-458a-85c9-c4f64dd89e9f.png">

## Basics of Cloud Native Technologies
### 1. Traditional/Legacy apps

Monolithic approach (self0contained and include all the functionality and components)
Pro: easy to develop and deploy
Con: hard to manage complexity, scale development across multiple teams, implement changes fast, scale the app out efficiently with a lot of load


### 2. Cloud native architecture 

break down your app in smaller pieces which makes them more manageable.


### 3. Characteristics of Cloud Native Architecture


* High Level of Automation\
  Achieved by using __Continuous Integration/Continuous Delivery (CI/CD) pipelines__ that are backed by a version control system like git. Building, testing and deploying apps and infrastructure with minimal human involvement allows for fast, frequent and incremental changes to production. Great for much easier disaster recovery.
* Self Healing\
  Monitor: health checks, compartmentalized applications
* Scalable\
  Application metrics: CPU or memory -> ensure availability and performance of your services
* Cost-Efficient\
  Scale down if traffic is low
* Easy to Maintain\
  __Microservices__ allows to break down apps in smaller pieces and make them more portable, easier to test and distribute
* Secure by default\
  Calls for different security models for different teams and users. For ex, __Zero trust computing__


### 4. Starting point example: [the 12-factor app](https://12factor.net/)

### 5. Knowledge Points


* Autoscaling\
  Horizontal scaling: spawning new compute resources (new copies). \
  Vertical scaling: Change in size within certain hardware limits. 
  > To illustrate the difference between scaling vertically and horizontally, imagine that you have to carry a heavy object that you cannot pick up. You can build muscle to carry it yourself, but your body has an upper limit of strength. That's vertical scaling. You can also call your friends and ask them to help you and share the work. That's horizontal scaling.
  Configurations: minimum & maximum limit of instances, metric that triggers the scaling -> tune -> run many load tests, analyze the behavior and load balancing while scaled.
* Serverless\
  Infrastructure as a Service\
  Platform as a Service\
  Function as a Service\
  <img width="921" alt="image" src="https://user-images.githubusercontent.com/24993672/210119211-2c3be7d5-3186-490f-953d-0e190e41678f.png">\
  __Serverless architecture example__
  <img width="970" alt="image" src="https://user-images.githubusercontent.com/24993672/210119489-191c2ab1-4b92-481b-87bd-0d6a9a530b04.png">
  > Writing small, stateless applications make them a perfect fit for event or data streams, scheduled tasks, business logic or batch processing.
* Open Standards\
  How to build and distribute software packages? Containers (with Docker) -- [Open Container Initiative](https://opencontainers.org/) (OCI)
  

## High level architecture of Kubernetes
## Container Orchestration (CO)
## How CO differes from legacy deployments 




## Deliver and monitor your app in a distributed system


