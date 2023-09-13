### Helm

<details>
<summary>What is Helm?</summary><br><b>

Package manager for Kubernetes. Basically the ability to package YAML files and distribute them to other users and apply them in the cluster(s).

As a concept it's quite common and can be found in many platforms and services. Think for example on package managers in operating systems. If you use Fedora/RHEL that would be dnf. If you use Ubuntu then, apt. If you don't use Linux, then a different question should be asked and it's why? but that's another topic :)
</b></details>

<details>
<summary>Why do we need Helm? What would be the use case for using it?</summary><br><b>

Sometimes when you would like to deploy a certain application to your cluster, you need to create multiple YAML files/components like: Secret, Service, ConfigMap, etc. This can be tedious task. So it would make sense to ease the process by introducing something that will allow us to share these bundle of YAMLs every time we would like to add an application to our cluster. This something is called Helm.

A common scenario is having multiple Kubernetes clusters (prod, dev, staging). Instead of individually applying different YAMLs in each cluster, it makes more sense to create one Chart and install it in every cluster.

Another scenario is, you would like to share what you've created with the community. For people and companies to easily deploy your application in their cluster.
</b></details>

<details>
<summary>Explain "Helm Charts"</summary><br><b>

Helm Charts is a bundle of YAML files. A bundle that you can consume from repositories or create your own and publish it to the repositories.
</b></details>

<details>
<summary>It is said that Helm is also Templating Engine. What does it mean?</summary><br><b>

It is useful for scenarios where you have multiple applications and all are similar, so there are minor differences in their configuration files and most values are the same. With Helm you can define a common blueprint for all of them and the values that are not fixed and change can be placeholders. This is called a template file and it looks similar to the following

```
apiVersion: v1
kind: Pod
metadata:
  name: {[ .Values.name ]}
spec:
  containers:
  - name: {{ .Values.container.name }}
  image: {{ .Values.container.image }}
  port: {{ .Values.container.port }}
```

The values themselves will in separate file:

```
name: some-app
container:
  name: some-app-container
  image: some-app-image
  port: 1991
```
</b></details>

<details>
<summary>What are some use cases for using Helm template file?</summary><br><b>

* Deploy the same application across multiple different environments
* CI/CD
</b></details>

<details>
<summary>Explain the Helm Chart Directory Structure</summary><br><b>

someChart/     -> the name of the chart
  Chart.yaml   -> meta information on the chart
  values.yaml  -> values for template files
  charts/      -> chart dependencies
  templates/   -> templates files :)
</b></details>

<details>
<summary>How Helm supports release management?</summary><br><b>

Helm allows you to upgrade, remove and rollback to previous versions of charts. In version 2 of Helm it was with what is known as "Tiller". In version 3, it was removed due to security concerns.
</b></details>

#### Commands

<details>
<summary>How do you search for charts?</summary><br><b>

`helm search hub [some_keyword]`
</b></details>

<details>
<summary>Is it possible to override values in values.yaml file when installing a chart?</summary><br><b>
Yes. You can pass another values file:
`helm install --values=override-values.yaml [CHART_NAME]`

Or directly on the command line: `helm install --set some_key=some_value`
</b></details>

<details>
<summary>How do you list deployed releases?</summary><br><b>

`helm ls` or `helm list`
</b></details>

<details>
<summary>How to execute a rollback?</summary><br><b>

`helm rollback RELEASE_NAME REVISION_ID`
</b></details>

<details>
<summary>How to view revision history for a certain release?</summary><br><b>

`helm history RELEASE_NAME`
</b></details>

<details>
<summary>How to upgrade a release?</summary><br><b>

`helm upgrade RELEASE_NAME CHART_NAME`
</b></details>
