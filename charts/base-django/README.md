# Django Helm Chart 

This chart's aim is to deploy a Django app using Helm. This chart follows the structure of the Django apps developed by 
France Université Numérique. Is is intended to be used as a subchart for Django app Deployments. 

This chart allows the user to either create a Postgresql database or connect to an external database (it depends on the variable `postgreqsl.enabled`)

To deploy your own Django app, create your own Helm chart, add this one as a subchart and overwrite its parameters as needed. To do this you have to add a section in your own values file as follows:

```
django:
  image:
    repository: fundocker/<app_image>
    pullPolicy: IfNotPresent
    tag: <image_tag>
  appDjangoSettingsModule: <settings_module>
  appDjangoConfiguration: <app_configuration>

```
Since we have not yet created our own helm chart repository, in order to use this chart as a subchart, you need to:

1) Go to this chart's directory: ` cd charts/base-django`

2) Build its dependencies: ` helm dependency build `

3) Package this chart: ` helm package . ` , a .tgz file will be created.

4) Copy the .tgz in `<your-chart>/charts` directory

It is possible to use existing Secrets or ConfigMaps for the deployment. In order to do this, you need to set the `extraEnvVarsSecret` and the `extraEnvVarsCM` variables with the name of the corresponding resource.

One other way to set the environment variables is to deploy custom Secrets and ConfigMaps in the parent chart. For this to work, you need to deploy the custom secret with the following annotations:

```
    "helm.sh/hook": pre-upgrade, pre-install
    "helm.sh/hook-weight": "-5"

```

For additional custom nginx configuration, this can be done using the `ingress.nginxServerSnippet` variable.