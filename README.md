# This is a Gitea Operator Deployment Ready Kustomized Repo

to Quick start the repo just

```Bash
# Login to your OpenShift cluster with a user that has cluster-admin privileges.
# Run the Following:

oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:openshift-gitops:openshift-gitops-argocd-application-controller
oc apply -k cd

```

this will create the following:

1. An ArgoCD Application that directs to the "<https://github.com/rhilconsultants/gitea-operator.git>" repository
2. An ArgoCD Project that will group all Gitea ArgoCD resources under it
3. A namespace named "gitea"
4. A Catalog Source with the Gitea Operator.
5. The Gitea Operator Subscription.
6. The Gitea Operator CR.
7. A HTTP Service to work internally with the Gitea Webhooks

## Understanding the Gitea Operator CR manifest

```YAML
kind: Gitea
apiVersion: pfe.rhpds.com/v1
metadata:
  name: gitea-demo  # Name of the Gitea instance
  annotations:
    argocd.argoproj.io/sync-wave: "4"
spec:
  giteaSsl: true # work with an https endpoint
  giteaAdminUser: opentlc-mgr # gitea administrator username
  giteaAdminPassword: "" # Gitea Administraor user name, recommand to leave empty, the operator will generator one and print it in the instance status section
  giteaAdminPasswordLength: 32 # number of charecters of the Adminisrator password
  giteaAdminEmail: opentlc-mgr@redhat.com # Administrator fake e-Mail
  giteaCreateUsers: true # Enable the operator to automate user creatation
  giteaGenerateUserFormat: "user%d" # User name template
  giteaUserNumber: 2 # Number of users to be created
  giteaUserPassword: openshift # Users Defualt password
  giteaMigrateRepositories: true # Allow live migration of exteranl repositories
  giteaWebhookAllowedHostList: '*' # Allow webhooks Hostlist
  giteaDisableSsh: false # Enable SSH usage with Repositorys
  giteaSshPort: 2022 # Gitea Server SSH Port, this is also need to be configured in the service.
  giteaStartSshServer: true # Start the SSH Server.
  giteaAllowLocalNetworkMigration: true # Allow to work with local network
  giteaRepositoriesList: # List of remote repo to be cloned to all users
  - repo: # Remote Repo URL
    name: # Remote Repo Name
    private: false # set the cloed repo as private / Public
```

More details can be found here: <https://github.com/rhpds/gitea-operator>

## Enjoy 🤪
