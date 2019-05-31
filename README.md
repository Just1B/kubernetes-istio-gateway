# Istio Gateway and VirtualServices on GKE

[![forthebadge](https://forthebadge.com/images/badges/made-with-go.svg)](https://forthebadge.com)

We will deploy 2 pods an dispach traffic with the Istio Gateway

Requirements

- A Cluster Already Online
- Istio on the cluster

## Step 1: Services

We will deploy 2 `hashicorp/http-echo` server an use the ingress to point on subdomains.

First create the fruits namespace

```
kubectl create namespace fruits
```

Deploy the http-echo servers

```
kubectl apply -f apple.yml
```

```
kubectl apply -f banana.yml
```

## Step 2: The Gateway

![index](https://github.com/Just1B/Kubernetes_Istio_Gateway/raw/master/screens/gateway.png)

Istio ingress gateway are like ingress controller, but unlike traditional ones, it does not rely on native Kubernetes Ingress objects. It uses its own custom resources: Gateway, VirtualService, DestinationRule, etc. Using custom resources provides more flexibility in configuring network policies

Let's deploy the gateway in the namespace `fruits` and listen on port 80

```
kubectl apply -f gateway.yml
```

Next we will need the gateway `EXTERNAL-IP` to point our domains

```
kubectl get svc istio-ingressgateway -n istio-system
```

Now you can point 2 subdomains ( TYPE A ) on the `EXTERNAL-IP`

Propagation can be long so wait for it 😴

## Step 3: VirtualService

We need to create a VirtualService for each of the services we want to expose, edit `virtual-service.yml`

```
kubectl apply -f virtual-service.yml
```

## Clear the Stack

You can delete the `fruits` Namespaces

# Licence

The MIT License

Copyright (c) 2019 JUST1B

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
