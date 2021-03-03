

#                       100 Istio Interview Questions

##                       By Mamun Rashid

###                       Linkedin:  https://www.linkedin.com/in/mamunrashid/

###                       Last Updated: 02.22.2021


##
##
#### 1. Why do we need Istio for time-outs, retries and circuit-breakers? 

  Answer: Without Istio (or some other service-mesh solution) , we would have to code these things in EVERY micro-service. If we have 200 micro-services, we would have to
       make sure that every single micro-service has these built-in.


##
#### 2.  Scenario Question: You have version 0.1 and 0.2 of an app running as two different Kubernetes Servcies. In Istio, what do you have to do, to send 90% of the traffic to version 0.1 and 10% to version 0.2 (Basically a Canary set up)?

  Answer: Define a VirtualService that has 2 destination rules. In the YAML, you can specify what percentage of traffic goes where.


##
#### 3.  Why do you need an Istio Gateway, when kubernetes already has an Ingress Gateway?

  Answer: Let us get some background first. The envoy proxies only come into play when containers talk to other containers in the cluster. That is when all our Istio configs 
  (e.g. splitting traffic between services) come into effect. However, that is NOT the case, when you have a front-end application on your cluster, because the incoming traffic 
  is from OUTSIDE the cluster.  In this case, we need an Ingress Gateway on the "edge" (i.e. "front gate"). That is exactly what Istio Ingress Gateway is. This "Istio Ingress 
  Gateway" has the ability to split incoming traffic and send them to different services at any chosen ratio.

##
#### 4. Scenario: You need to route traffic based on some criteria. For example, if the sender has a cookie set, send traffic to destination A. Or, if the URI has certain prefix, send traffic to destionation B. How do you accomplish this in Isto?

   Answer: Define a Gateway that TWO "match" sections. Each "match" section would implement a criteria-> destination.

##
#### 5. Scenario: For the example criteria match (e.g. incoming traffic URI has a particular prefix), you want to SPLIT the traffic to TWO different destinations.  How do you implement this scenario in Istio?

   Answer: Make a Gateway. In the YAML definition of the Gateway, define a criteria in the "match" section. THEN, in the "route" section for that match, simply define
           two different destinations (e.g. host)

##
#### 6. Scenario: You have a monolith. You want to chip away at this monolith slowly. You define a microservice A that handles a small part of monolithic service, while letting REST of monolith be as is. How can Istio help you in this situation?

######   Answer: 
######           1. Define ONE big VirtualService that points to the monolith.
######           2. Create a microservice (a service in Kubernetes) that handles a small part of the monolith
######           3. Now, edit the VirtualService to add the new microservice that handles the relevant requests (e.g. a REST path) 
######           4. Now, users still hit same the VirtualService and yet , behind the scene, Istio forwards ONLY the relevant requests to the brand new micro-service.
######           5. You can continue steps 2-4 for each small piece that you chip away from the monolith until the entire monolith has been converted to N micro-services.


##
#### 7. The "hosts" field in the definitions of VirtualService and Gateway is very powerful. Why?

   Answer: It represents the client's destionation. However, Istio allows this to take many forms, such as:
    
         - short form of Kubernetes Service Name
         - IP
         - FQDN
         - * wildcard
         and many more.


##
#### 8. How does Istio help with security?

   Answer: Istio can encrypt microservice to microservice traffic very easily


##
#### 9.  What core "thing" makes Istio possible?

   Answer: It puts an envoy proxy in each pod. This makees everything else in Istio possible.


##
#### 10. Can you name some of the add-ons for Istio?

   Answer: Kiali, jaeger, Grafana, Prometheus, Zipkin, Cert-Manager


##
#### 11. Can you think of a tool that helps us automate canary deployments on Kubernetes via Istio?

   Answer: Falgger  (open-source)


##
#### 12.  How do you access kiali, once instaleled?

  Answer: Just port forward to kali pod 20001


##
#### 13.  How does Istio does what it does?

  Answer: It installs sidecars along with application containers ("envoys") that intercepts and proxys traffic.


##
#### 14.   Is there such a thing as "Istio Ingress Controller"

    Answer: Yes. Istio supports standard Kubernetes Ingress specification without annotations


##
#### 15.  What does Istio install automatically in each pod ?

    Answer: Envoy

##
#### 16.  Can Istio do API Management at the edge?

 Answer: Yes


##
#### 17.   What is Istio routing?

  Answer: You can use Istio to send 60% traffic to version 1 of a microservice and 40% to version 2.  You can also route traffic based on conditions.


##
#### 18.  In terms of help with Micro-Service development, does Istio support retry?

  Answer: Yes

##
#### 19.  In terms of help with Micro-Service development, does Istio support time-out?

  Answer: Yes

##
#### 20.  In terms of help with Micro-Service development, does Istio support circuit-breakers?

  Answer: Yes


##
#### 21.  Can microservice to microservice communication "leave the building" ??

   Answer: Yes.  This is why you need encryption between microservices.


##
#### 22.  In terms of help with Micro-service development, does Istio support Load Balancing?

  Answer: Yes


##
#### 23.  In terms of help with Micro-service development, does Istio support Fault Injection for testing?

  Answer: Yes

##
#### 24.  Does Istio support redirect?

  Answer: Yes

##
#### 25.  Does Istio support re-write?

  Answer: Yes. (re-writes paths on the fly)


##
#### 26.  What is Istio re-write?

  Answer: Istio can changes the header of the incoming requests. e.g. /foo/bar/a  TO /a

##
#### 27.  Can you name couple of reporting features of Istio?

 Answer:  1. Telemetry 2. Distributed Tracing and 3. Logging


##
#### 28. Does iatio have policy enforcement feature?

 Answer: yes


##
#### 29.  Can you do dark releases using Istio (to your PRODUCTION environment)? 

  Answer: YES!


##
#### 30.  Can you name one way you can deploy dark release into your PRODUCTION Environment using Istio?

  Answer: Istio can re-direct traffic based on headers. You can have a micro-service (or a new version of it) that responds only when the HEADER is present.
       You, as a tester (or an automated testing application) can send a request with the added header and Istio will re-direct those to the new version of the Micro-Service.


##
#### 31.  In Istio, how can you suspend traffic between Micro-Service A and B?

  Answer: In Kiali GUI, you can "suspend" traffic between services using mouse-clicks.


##
#### 32.  What is a named subset and how does that fit into the big picture?

  Answer: Named subset is an endpoint. Endpoint, for example, can be pods with certain labels.
       Destination Rules has Named Subsets. So, in the big picture, Named Subsets are where traffic end up after all said and done.


##
#### 33. Name one of the new k8s object that Istio creates:

  Answer: virtualservice and destinationrules


##
#### 34.  Which Istio feature allows us to track a single request?

   Answer: Distributed Tracing


##
#### 35.  Let's say you have Flagger deploy canary to your environment. But, this is just for demo and there is no traffic coming in. What will Flagger do?

   Answer: It will think that Canary Deployment has failed and it will roll back.


##
#### 36.  Name one distrubuted tracing framework that comes with Istio:

   Answer: Jaeger


##
#### 37.  In Distribute Tracing  ONE Trace has multiple ______________(s):

   Answer: span


##
#### 38.  What exactly is a "span" in a Trace?

   Answer: If a request hops from point A to B to C to D,  AB , BC and CD are Spans.


##
#### 39.  Jaeger is basically what looking for tracable requests ?

   Answer: Search Engine


##
#### 40.  Can you name some of the features of Istiod (formerly known as Pilot)?

   Answer: Routing  +  timeout,retries,circuit-breakers,load-balancing,fault-injection


##
#### 41.  What does flagger do?

  Answer: promotes canary based on metrics , little by little (10% --> 20% --> etc) and rolls back if metrics don't look good.


##
#### 42.  Can you name another service-mesh besides Istio?

  Answer: Linkerd


##
#### 43.   How to install Istio ? 

  Answer: Version 1.9 (2021) curl down the Istioctl and then use Istioctl to install Istio


##
#### 44.  What manages certificates in Istio?

    Answer: CA (Certificate Authority)


##
#### 45.  What is Citadel?

  Answer: Old name of CA:


##
#### 46.  Envoy , sidecar proxy, gets its routing and configuration from __________ :

   Answer: Pilot/Istiod


##
#### 47.  What is this "Mixer" thing in Istio?


######   Answer: 
######   source:  https://Istio-releases.github.io/v0.1/docs/concepts/policy-and-control/mixer.html
######        Mixer provides a generic intermediation layer between application code and infrastructure backends. Its design moves policy decisions out of the app layer and into configuration instead, under operator control. Instead of having application code integrate with specific backends, the app code instead does a fairly simple integration with Mixer, and Mixer takes responsibility for interfacing with the backend systems.


##
#### 48.  Which 3 Core Services does MIXER provide?

  Answer:  Pre-condition Checking, Quota Management, Telemetry Reporting


##
#### 49.  Envoy gets certificates from ________________

   Answer: CA (Formerly named Citadel)


##
#### 50.   Does Istio has both ingress and egress controllers?

   Answer: True


##
#### 51.  How is Istio's VirtualServices different than a normal k8s service:

  Answer: Has additional features such as:
       a. external route management
       b. external traffic management
       c. pod to external communication
       d. HTTPS external communication
       e. routing
       f. URL rewriting


##
#### 52.   Can you do canary deployment with plain kubernetes?

  Answer: Yes . it is ugly. You make 2 services taregtting different labels. Pods will have those different "tags".


##
#### 53.  Can Istio do canary release?

  Answer: Yes


##
#### 54.  Does Istio have a control pane?

  Answer: Yes

##
#### 55.  In which namespace does the control pane live?

  Answer: Istio-system


##
#### 56.  Can you use Istio to encrypt traffic between micro-services ?

  Answer: Yes


##
#### 57.  Name one advantage of Istion in terms retries?

  Answer: if MS A calls MS B. You will have to incorporate retry login for this call in your app. Same for every single MS to MS communications.
       Istio can do this for you.


##
#### 58. With Istio, how can you prevent man in the middle attack?

  Answer: My using encryption between microservices

##
#### 59. What is the difference between a micro-service and kubernetes service?

  Answer: micro-service is a generic term for a "piece" of an application. A Kubernetes service is a front for a bunch of pods doing the same thing.  However, in real-life, often a micro-service is implemented as a Kubernetes service.


##
#### 60.  What is mutual TLS?

  Answer: In a normal browset:web-server relationship, browser verifies the server using SSL/TLS. But Istio is capable of doing TLS both ways between micro-services.
       This is called mutual TLS


##
#### 61.  If Micro-Service A calls Micro-Service B, but not the other way around , which service should implement circut breaker?

  Answer: A


##
#### 62.  Why would you need a circuit-breaker?

######  Answer:  To avoid casdaing failure.
######
######        Let's say Micro-service A calls Micro-Service B and Micro-Service B calls Micro-Service C.
######         A  ---->   B   ---->  C
######
######        For example, if Micro-service C (called) has issues and can't server Micro-service B (caller) would have problems servicing  Micro-service A. Then Micro-service A 
######        Fails, as well.  To avoid this cascading failure, A and B should implement circuit-breaker.


##
#### 63.  What does Jaeger add-on do?

   Answer: Distributed Tracing


##
#### 64.  What are Istio profiles?

  Answer: Although this feature has been becoming less and less relevant, profiles dictates what parts of Istio gets installed when you install Istio.


##
#### 65.  How does Kiali show "encrypted" communication between MSs?

   Answer: You see a lock just like you see next to URL on your browser


##
#### 66.  Why are companies shying away from Istio in prod at this point (2021)

  Answer: It is still beta. For example, Upgrading Istio version (when already is use) is not a reliable process.


##
#### 67.  Can you use Istio to make one of your Micro-Service response with an artificial delay?

  Answer: Amazingly, YES. This is one type of Fault Injection that Istio Supports.


##
#### 68.  Sometimes we hear the term Telemetry when it comes Istio and/or Kubernetes. What is Telemetry in this context?

  Answer: Within Istio and Kubernetes, there are ways to look into myriad of metrics that helps us understand what is really happening inside our cluster. These metrics make up "Telemetry" 


##
#### 69.  You already have a Kubernetes Service in you Cluster. You also have Istio installed. What is an easy way to get an YAML for a VirtualService (that does not yet exist) that points to your existing Kubernetes Service?

######  Answer: 
######          In Kiali, find the service that you are interested in. 
######          Right Click on the service.
######          Show details
######          Actions
######          Suspend Traffic
######          Create
######          Click on YAML Tab.


##
#### 70. When you install Istio, how does Istio know which namespaces are fair-game to install proxies into?

  Answer: Namesspaces that have a label Istio-injeection=enabled will get the proxies.


##
#### 71. In Istio, What is a Gateway?

  Answer: It is Load Balancer that sits on the edge and handles incoming and outgoing traffic.


##
#### 72. How to you control traffic coming in and going out of a VirtualService?

  Answer: Attach it to Gateway (Which is a Load Balancer)


##
#### 73. You are defining a "Gateway" using a YAML file. How do you let this Gateway accept traffic multiple ports (e.g. 80,443,1443)

 Answer: 

   Just like any other kubernetes YAML file, you would have a "list" by having multiple lines that start with dash.

   For example:

  - port:
      number:80
      name: http
      protocol: HTTP
    hosts:
    - "*"
  - port:
      number: 443
      name: SSL
      protocol: HTTPS
    hosts:
    - "*"


##
#### 74. What is Kiali used  for in Istio?

   Answer: Its the graphical interface showing traffic between micro-services in real time (plus more) allowing real-time fault finding easy.


##
#### 75.  Is Istio application-aware?

  Answer: Yes, to some-extent


##
#### 76. Can you explain what is a service-mesh?

  Answer: From 20,000 feet view, a service-mesh is an abstraction layer on top of all services/micro-services that run on Kubernetes. This helps us do many things with interc-connected
       Kubernetes services that would be extremely "manual" or outright impossible. For example, Canary deployment, or time-out or circuit-breaking.


##
#### 77.  What is important about Google backing Istio?

  Answer: Kubernetes was made by google.


##
#### 78. You have defined several Routing Rules. Which will take precedence?

   Answer: They are evaluated in order (top to bottom)


##
#### 79. Scenario: You have version 1 of your microservice hosted on Kubernetes Service foo. You have version 2 of your microservice hosted on Kubernetes Service bar. You want to send 10% of 
     incoming traffic to Service bar. What you be simple Istio Solution without using Flagger.

   Answer: Define a Virtual Service which has two destinations (foo and bar). Give "foo" 90% weight and "bar" 10% weight.



##
#### 80. Istio's VirtualService is pretty darn good at devising routing policies to match just about any scenarios. Why then we still need "Gateways" ?

   Answer: Gateways live on the "edge" of your mesh. This allows it to manage incoming traffic.


##
#### 81. Which takes effect first? Routing Rules or Destination Rules?

  Answer: Destination Rules come AFTER Routing Decisions have already been made.


##
#### 82. Given that the functional testing of new feature is difficult to be tested automatically, can you think of couple of alternative tests you can run on your micro-service in an automated fashion?

  Answer: Two relatively easy tests that can automated are: Error rate and Latency. By comparing these metrics before and after a Canary Release, you can get some confidence in your ability to evalute how the Canary deployment went.


##
#### 83. Imagine that you have magic tool that is able to deploy Canary deployments bit by bit (10%, 20%, 30% and onwards) on your production cluster automatically. What is the pre-requisite
    for this tool to succeed?


  Answer: Tool must have an automated way to check the health of the services (e.g. error rate or latency) at every stage. This will allow it to make the right decision (go forward or rollback).  By the way, such a tool exists. It's called Flagger and it is open source. 


##
#### 84. Is the side-car pattern the only way to implement service-mesh?


##
  Answer: No. Client-side Libraries (e.g. Hysterix ) can also be used. In that case, these libraries perform same function as the data plane of Istio. Netflix is a good example of a company who has adopted this approach.


##
#### 85. What is CRD and how does that relate to Istio?

  Answer: CRD Stands for Custom Resource Definitions. Just Kubernetes creates "objects" using declarative YAML files, Istio creates its own objects using YAML such as VirtualService and DestinationRule.


##
#### 86. What is Strangler Pattern and how does it relate to Istio?


######  Answer: 
######          A great explanation is give here.
######          https://martinfowler.com/bliki/StranglerFigApplication.html
######          In summary, you chip away at the monolith until you have replaced all of it using N endpoints or micro-services.
######          In Istio, VirtualService helps you accomplish that. 


##
#### 87. What is a Service Entry in Istio?

  Answer: Istio has an internal registry of services. Each item in this registry has a way reach that service. Each item in the Service Registry is a Service Entry.


##
#### 88. Why would you need Service Entries in Istio Service Registry when Istio and Kubernetes are already aware of their services?

 Answer: Mostly the service registry is a place to have definitions of services outside the Istio and Kubernetes. This allows service within Istio to easily reach out to those extrenal
         services (define once, use many times).


##
#### 89. What would a good example use case of a Service Entry?


  Answer: For example, If multiple services within your Kubernetes Cluster are reaching out to an external service that requires mTLS, Istio's service entry can be used
          as a proxy. This would nullify the need to configure mTLS on every single service within your cluster. You would just configure it once.


##
#### 90. Can you modify the configurations of the side-car proxies that get injected into all prods by Istio?

  Answer: Yes. By default, side-car proxy grabs everything and allows all traffic in and out of pods. This behavior can be changed by hand.


#### 91. Once you install Istio in your cluster, how do you know if it is running?

  Answer: You can look for the Istio pods using the following command:

          kubectl get pods -n Istio-system

#### 92. Scenario: You have a YAML file that can deploy your APPLICATION pods. These pods go into "foo" namespace. The foo namespace has Istio-enabled turned on. Is this enough for your
      application to be monitored/covered by Istio?

    Answer: No! Istio does not auto-enforce proxies in to any pods.

#### 93. Scenario: You have a YAML file (foo-app.yaml) that can deploy your APPLICATION pods. These pods go into "foo" namespace. The foo namespace has Istio-enabled turned on. What do you have to do
     to have your application be Istio-managed as soon as you deploy.

    Answer: Use Istio's kube-inject feature to your YAML file BEFORE deploying. For example:

    Istioctl kube-inject -f foo-app.yaml > foo-app-with-Istio.yaml
    kubectl apply -f foo-app-with-Istio.yaml

 
#### 94. If you have a virtualservice named foo, how do you get it's configuration?

  Answer: kubetcl get vs foo -o yaml

#### 95. If you have a Gateway named foo, how do you get it's configuration?

  Answer: kubetcl get gw foo -o yaml

#### 96. If you have a Istio Gateway, how do you figure out it's external IP so that you can access it?

  Answer: Under the hood, the Istio Gateway is using kubernetes service. If you describe that service (kubectl get svc service_name) , you will see what kind of a Load Balancer
          It is using and it's IP. For example, in AWS, it will be a ALB/NLB/ELB.


#### 97. Which UI let's you gives you the chance to distributed tracing to see traffic flow of the requests to your application?

  Answer: Zipkin UI

#### 98. How do you access Zipkin UI?

  Answer: You have to port-forward to zipkin pod

#### 99. If you want to send 25% traffic to application version 1 (pods with label v1) and 75% to application version 2 (pods with label v2), which object do you use?
    Gateway, VirtualService or DestinationRule?

  Answer: VirtualService


#### 100. What is the mechanism that Istio provides that allows us to authenticate users to a service?

  Answer: This is called Origin Authentication. This is done using JWT Tokens.

 


