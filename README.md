# Openshift Template for Clair

This template is to implement clair inside Openshift.  

## What's inside

Clair : quay.io/coreos/clair:latest  
Postgres: Postgres 9.5 (Persistent Storage)  

## How to Deploy

- Login into Openshift Console.  
- Click 'Import from YAML/JSON'  
- Paste the contents of template.yml file.
- Create > Change the parameters if needed.

If you want to customise the config.yml of clair, you can edit it in configMap inside the template or in Openshift.

To crate the app for the first time, `oc new-app -f template.yml`.

> Wait for few minutes for the application to update and populate the Database. Please ensure that Clair pod can communicate with the addresses mentioned in `egress-policy.yml`.

> If you are using klar, you will need to specify the port manually '80'.

## How to update changes

Use the below command to update the existing installation after updating the template. Below command can be used in pipeline as this command will also create app if not exists.

`oc process -f template.yml | oc apply -f -`

## Egress network policies & Firewall

If your Openshift cluster is secured with network policies and firewall. Please ensure that egress network policies are applied to the namespace and the firewall. You might also want to open your firewall from the policy.

`oc process -f egress-policy.yml | oc apply -f -`

> Please ensure to add your Docker repository URL to the list.

> `ovs-networkpolicy` SDN plugins allow to have only one Egress policy. In that case, please update the existing egress policy with this rules.

## How to destroy

Use the below command to destroy the clair installation completely.

> Below command will also destroy the database volume. You can remove `pvc` from the command, if you want to keep the database files.

`oc delete all,cm,pvc -l app=clair`