---
title: "What is Blackbeard?"
date: 2018-07-11T12:42:17+01:00
anchor: "introduction"
weight: 10
---

**Blackbeard is a Kubernetes namespace manager.** It helps you to develop and test with Kubernetes using namespaces.

{{% block tip %}}
Kubernetes namespaces provide an easy way to isolate your components, your development environment, or your staging environment.
{{% /block %}}

Blackbeard helps you to deploy your Kubernetes manifests on multiple namespaces, making each of them running a different version of your microservices. You may use it to manage development environment (one namespace per developer) or for testing purpose (one namespace for each feature to test before deploying in production).

## Purpose

*When working in a quite large team or, in a quite large project, have you ever experienced difficulties to test multiple features at the same time?*

Usually, teams have 2 alternatives :

* Stack "features to test" in a queue and wait for the staging environment to be available;
* Try more or less successfully to create and maintain an "on demand" staging environment system, where each environment is dedicated to test a specified feature.

Blackbeard helps you to create ephemeral environments where you can test a set of features before pushing in production.

It also provide a very simple workflow, making things easier if you plan to run automated end-to-end testing.

## Playbook

Blackbeard use *playbooks* to manage namespaces. A playbook is a collection of kubernetes manifest describing your stack, written as templates. A playbook also require a `default.json`, providing the default values to apply to the templates.

Playbooks are created as files laid out in a particular directory tree.

## Usage

Blackbeard provide a CLI interface in addition to a REST API. This way, you can use it either to run automated tests in a CI pipeline or plug your own UI for manuel deployment purpose.

#### Creating a new isolated env

```sh
blackbeard create -n my-feature
```

This command actually *create a namespace* and generate a JSON configuration file containing default values. This file, called an `inventory`, is where you may update the values to apply specifically to your new namespace (such as microservice version)

#### Applying changes

```sh
blackbeard apply -n my-feature
```

This command apply your kubernetes manifest (modified by the values you have put in the generated `inventory` previously) to your newly created namespace

#### Getting services endpoint / ports

```sh
blackbeard get services -n my-feature
```

This step will prompt a list of exposed services in the namespace. If you need to connect to a database in order to test data insertion, it is where you will find useful info.

#### Getting back to previous state

```sh
blackbeard delete -n my-feature
```

Delete all generated files and delete the namespace.