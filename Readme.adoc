= Deploy nginx ingress controller on kubernetes
Uniscon GmbH <support@sealedplatform.com>
v1.0, 2019-06-28
:sp-caption!:
ifndef::imagesdir[:imagesdir: images]
:title-logo-image: image:uniscontuevlogo.png[align=center,pdfwidth=50%]

This guide will walk you through deploying nginx ingress on Sealed Platform. The guide assume that the reader is already registered with Sealed Platform as a Tenant user, and can login to the Dashboard. It is also strongly recommended to go through the Developer's guide for detailed instructions.

== Step 1 : Prepare the default-server.secret.yaml with your domain certs

Login to the Sealed Platform dashboard as the Tenant admin user. Navigate to 'Servers' section, click on a listed server, under the Certificates download "Chain PEM" and "Key PEM"  parts to your local.

Generate base64 encoded strings without line wrapping for <base64-cert-pem> and <base64-key-pem> respectively, from .pem files

 $ base64 -w 0 <YourDomain_chain.pem>
 $ base64 -w 0 <YourDomain_key.pem> 

Update default-server.secret.yaml file <base64 -w 0 cert_chain.pem>, <base64 -w 0 cert_key.pem> with above base64 encoded strings. 


== Step 2 : Prepare the nginx-ingress.service.yaml file with your assigned server InternalIP.

Login to the Sealed Platform dashboard as the Tenant admin user. Navigate to 'Servers' section, click on a listed server, find the Internal IPs.

Update nginx-ingress.service.yaml file <Internal IP> with Internal IP.

== Step 3 : Apply the Nginx-ingress yaml files in below order.

 $ kubectl apply -f default-server.secret.yaml
 $ kubectl apply -f nginx-config.configmap.yaml
 $ kubectl apply -f nginx-ingress.deployment.yaml
 $ kubectl apply -f nginx-ingress.service.yaml

Verify the nginx ingress controller with https://<DomainName> , it should respond with default 404 error , as it does not have any backend services running.

NOTE: In this documentation we are using nginx/nginx-ingress:1.5.6 image, you can update with latest image or continue with the same image.
