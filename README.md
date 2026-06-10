# GitOps Repository

## Overview
This repository contains the ArgoCD GitOps configuration for the Flask AWS Monitor application.

ArgoCD uses an ApplicationSet to generate separate deployments for three environments:

- dev
- qa
- prd

## Repository Structure

- charts/flask-aws-monitor - Helm chart used by ArgoCD
- flask-aws-monitor/dev/values.yaml - Development configuration
- flask-aws-monitor/qa/values.yaml - QA configuration
- flask-aws-monitor/prd/values.yaml - Production configuration
- yamls/flask-applicationset.yaml - ArgoCD ApplicationSet
- yamls/gitops-project.yaml - ArgoCD AppProject

## Environment Differences

| Environment | Replicas | Service Type |
| --- | --- | --- |
| dev | 1 | ClusterIP |
| qa | 2 | ClusterIP |
| prd | 3 | LoadBalancer |

## Deployment

kubectl apply -f yamls/gitops-project.yaml
kubectl apply -f yamls/flask-applicationset.yaml

## Validation

kubectl get applications -n argocd
kubectl get namespaces

Changes pushed to this repository are automatically synchronized by ArgoCD.
