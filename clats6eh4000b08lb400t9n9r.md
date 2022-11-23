# Validate, Clean & Secure K8s YAML files

## Introduction

Kubernetes workloads & configurations are most commonly defined in the YAML format / YAML files. Configuration in K8s can sometime become complicated for a developer with the pod and container configuration files. When those manifests scale in size you could easily end up overlooking a configuration option which could turn out to cost you some good bucks. One of the major challenges with YAML is to express constraints and the relationships between the manifest files. 

**What if you wish to check that all images deployed into the cluster are pulled from a trusted registry?
**

**How can you prevent Deployments that don't have PodDisruptionBudgets from being submitted to the cluster?
**

Integrating static checking allows catching errors and policy violations closer to the development lifecycle and since the confirmation around the validity and safety of the resource definitions is improved you can trust that production workloads are following best practices.

In this article we'll not only dive into validation of YAML but also will understand the best practices of K8s development.

This article is intended as a guide for validating K8s manifest files. If you are a developer, devops engineer, devops specialist or simply interested in learning more about the quality assurance process around creating and managing K8s manifests then this article is for you.

## Getting Started

Kubernetes already has a powerful and segreggates process of validating resources in the form of the admission controllers, policy enforcement via tools like Open Policy Agent, Kyverno or Kubewarden, pod security policies (deprecated in 1.21) etc

This tools are great but they derive from the cluster operator layer and are more aligned on a platform rather than a tool that fits neatly in a standard developer workflow.

The tools and methods we learn in this article will transfer the kick-off point of a validation process to a development workflow and thus greatly improve the quality of the whole ecosystem.

**The focus here is to make illegal states irrepresentable. This is difficult to do with a non-statically typed language like YAML but with the right tools it can be done.
**

### What you will learn

After reading this article you will learn:

- Different ways to validate K8s YAML files
- Benefits of automating the validation process in a CI/CD pipeline
- Tools used in validating different kinds of YAML files

### Prerequisites

If you would like to follow along and get your hands dirty with YAML, you will need:

- VS Code with remote development extension
- Docker desktop or any of the alternatives (need to be able to run containers)
- Clone this repository

The ecosystem of static checking of K8s YAML files can be grouped in the following categories:

**API validators —** Tools in this category validate a given YAML manifest against the Kubernetes API server.

**Built-in checkers —** Tools in this category bundle opinionated checks for security, best practices, etc.

**Custom validators —** Tools in this category allow writing custom checks in several languages such as Rego and Javascript.


### What do we need to validate?

Let's try to break down the validation process into categories and understand how following the same distinction can help you validate your yaml files faster than ever?

**Structural validation:** Maintaining the yaml file structure can be a bit mess sometimes for the developer thus validating the structure before execution can help scale down error encountering probability to some extent.

**Semantic validation of K8s schema:** Here the focus is given on validating whether the file is a correct K8s YAML file. This is an automated process is a bit too late in the lifecycle. An interesting tool that we'll explore in this space is kubeval.

**Pragmatic validation of the resource:** This is where the validation process looks at the file from different contexts. We want to check the file and configuration for security vulnerabilities, performance issues, adherence to best practices, versioning schemes and many more. We'll check out two tools trivy and datree. Both bringing a unique perspective and functionality.

In this article we'll explore 4 different tools that can help us validate YAML files but there are so many more in the marketplace. Feel free to check  & play around the different tools available.

### Kubeconform

Kubeconform is a Kubernetes manifest validation tool. Incorporate it into your CI, or use it locally to validate your Kubernetes configuration!

![kube-conform](https://user-images.githubusercontent.com/19731161/142411871-f695e40c-bfa8-43ca-97c0-94c256749732.png)


It is inspired by contains code from and is designed to stay close to Kubeval but with the following improvements:

- High performance: will validate & download manifests over multiple routines, caching - downloaded files in memory
- Configurable list of remote, or local schemas locations, enabling validating K8s custom resources (CRDs) and offline validation capabilities
- Uses by default a self-updating fork of the schemas registry maintained by the kubernetes-json-schema project - which guarantees up-to-date schemas for all recent versions of Kubernetes.

[**Checkout the installation guide here**](https://github.com/yannh/kubeconform#Installation)

The fact that kubeconform has all the schemas for all the different Kubernetes versions available—and also doesn't require Minikube setup (as kubectl does)—makes it a superior tool when comparing these capabilities to its alternatives.

### Kube-score

Kube-score is a tool that performs static code analysis of your Kubernetes object definitions. The output is a list of recommendations of what you can improve to make your application more secure and resilient.

You can test kube-score out in the browser with the [online demo](https://kube-score.com/) ([source](https://kube-score.com/)). Kube-score checks are an excellent tool to enforce best practices but currently it does not support customization or addition of custom rules.


![kube-score](https://user-images.githubusercontent.com/47952/56085330-6c0a2480-5e41-11e9-89ba-0cfddd7714a8.png)

If you want to write custom checks to comply with your organisational policies you can use one of the next two options - config-lint or polaris.

### Config-lint

Config-lint is command line tool used to validate configuration files using rules specified in YAML. The configuration files can be one of several formats: Terraform, JSON, YAML, with support for K8s. There are built-in rules provided for Terraform and custom files can be used for other formats.

[**Check out the demo here**](https://stelligent.github.io/)

Config-lint comes with no in-built checks for Kubernetes manifests which means you'll have to write your own custom rules to perform any validations.

The rules are written as YAML files referred to as rulesets and have the following structure:

```
version: 1
description: Rules for Kubernetes spec files
type: Kubernetes
files:
  - "*.yaml"
rules:
   # list of rules
```

[**Check out the latest release version here**](https://github.com/stelligent/config-lint/releases/tag/v1.6.0)

### Polaris

Polaris is an open source policy engine for Kubernetes that validates and remediates resource configuration. It includes 30+ built in configuration policies, as well as the ability to build custom policies with JSON Schema. When run on the command line or as a mutating webhook, Polaris can automatically remediate issues based on policy criteria.

![polaris](https://camo.githubusercontent.com/21017bcdf60b658e5719e8d4b8ebf4ba4c1115ea907f2d8190427a82f8979eaf/68747470733a2f2f706f6c617269732e646f63732e6661697277696e64732e636f6d2f696d672f706f6c617269732d6c6f676f2e706e67)

Polaris can be run in three different modes:

- As a dashboard: Validate Kubernetes resources against policy-as-code.
- As an admission controller: Automatically reject or modify workloads that don't adhere to your organization's policies.
- As a command-line tool: Incorporate policy-as-code into the CI/CD process to test local YAML files.

Similar to kube-score, polaris identifies several test cases where the manifest falls short of recommended best practices which include:

- Pods health-check.
- Unspecification of tags in container images.
- Container run status(i.e root).
- CPU and memory requests and limits are not set.
- Checks are classified with a severity level of warning or danger.

[**To learn more about the current in-built checks refer to the documentation.**](https://polaris.docs.fairwinds.com/)

### Conclusion

While there are a bunch of tools to validate, score and lint K8s YAML files it becomes important to have a structural model on how you want to design and perform the checks.

The following table presents a summary of the tools:

| Tool Name | Features | Constraints | Custom Checks 
| --- | --- | --- | 
| Kubeconform | Validates & downloads manifests over multiple routines, caching - downloaded files in memory | Kubeconform, similar to kubeval, only validates manifests using the official Kubernetes OpenAPI specifications | No 
| Kube-score | Analyses YAML manifests against standard best practices Deprecated API version check | Doesn't validate the definition No support for specific API versions for deprecated resource check | No
| Config-lint | A generic framework for writing custom checks using DSL embedded in YAML The framework also supports other configuration formats - Terraform, for example. | No inbuilt tests The inbuilt assertions and operations may not be sufficient to account for all checks | Yes
| Polaris | Analyses YAML manifest against standard best practices Allows writing custom checks using JSON Schema | JSON Schema-based checks may not be sufficient | Yes

These tools are not dependent on access to a K8s cluster are easier to set up with allowing you to implement gating as well as give quick feedback to pull request authors for projects.

Feel free to check out the tools and share the article if you find it useful. Thanks.



