# Purpose of this repository

This repository is for a test to show HTTP headers in requests passed through Azure API Management.

## Prerequisites

- Azure Kubernetes Service cluster
- Azure container Registry
- ACR attached to AKS
- Azure AD B2C
- Azure API Management

## Basic architecture

```
[Client]--HTTPS--[APIM]--HTTP--[LB]--HTTP--[AKS [Pod:nginx-with-openresty]]
```

# Docker image

The Dockerfile in this repository is for an image of OpenResty, an Nginx module for lua script, with a configuration to dump HTTP headers to error log.

## Build

```
$ docker build -t xxx/openresty:v1 .
```

## Push

```
$ docker tag xxx/openresty:v1 xxx.azurecr.io/xxx/openresty:v1
$ docker push xxx.azurecr.io/xxx/openresty:v1
```

# Test

## Deploy

```
$ kubectl apply -f deployment.yaml
$ kubectl apply -f service.yaml
```

## Show logs

```
$ kubectl logs nginx-xxxxxxxxxx-xxxxx -f
```

## Send request to APIM

```
curl -H "Ocp-Apim-Subscription-Key: $APIM_SUBSCRIPTION_KEY" -H "Authorization: Bearer $JWT" https://aks-portal-apim.azure-api.net
```

# References

- [OpenResty:https://openresty.org/en/]
- [Lua script to dump HTTP headers:https://www.greptips.com/posts/1271/]
- [Azure AD B2C Preparation:https://docs.microsoft.com/ja-jp/azure/java/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc]
- [Azure API Management AD B2C integration:https://docs.microsoft.com/ja-jp/azure/active-directory-b2c/secure-api-management?tabs=applications]
