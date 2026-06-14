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

## Local ArgoCD Validation

ArgoCD was installed and validated successfully on a local Docker Desktop Kubernetes cluster.

The ApplicationSet generated three applications:

- flask-aws-monitor-dev - Synced and Healthy
- flask-aws-monitor-qa - Synced and Healthy
- flask-aws-monitor-prd - Synced and Healthy

Validated environment behavior:

- dev - 1 replica with ClusterIP service
- qa - 2 replicas with ClusterIP service
- prd - 3 replicas with LoadBalancer service

The production application was successfully accessed through http://localhost:5001 and returned HTTP 200.

## Final Project Notes

This repository is used as the GitOps source for the final DevOps project.

ArgoCD reads this repository and deploys the Flask application to Kubernetes.

Main deployment file:
yamls/flask-applicationset.yaml

The ApplicationSet creates three environments from the same Helm chart:

| Environment | Replicas | Service Type |
|-------------|----------|--------------|
| dev | 1 | ClusterIP |
| qa | 2 | ClusterIP |
| prd | 3 | LoadBalancer |

Helm chart location:
charts/flask-aws-monitor

Environment values files:
flask-aws-monitor/dev/values.yaml
flask-aws-monitor/qa/values.yaml
flask-aws-monitor/prd/values.yaml

During the local validation, ArgoCD created the three applications and the production service returned HTTP 200 from localhost:5001.

