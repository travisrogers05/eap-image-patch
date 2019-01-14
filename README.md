## Example of creating an Openshift container using s2i based on JBoss EAP and a binary (WAR) file, that also includes a patched/updated EAP module.

#### Issue these commands from CLI to ensure you are not the system:admin, unless you mean to be.  Also that you are not operating on the default project.
```
oc login -u <some_user>
oc whoami
oc project
```

#### Create a project containing an EAP based application.  Use the web console or the supplied CLI commands for 1-3.  Use the CLI for 4.

1.  Create a new project (replace **project-name** with a name you choose)

  ```
  oc new-project <project-name>
  ```

2.  Add an application to the project.  Use the EAP 7.1 template image.  Provide the following fields for this example app:

  ```
  SOURCE_REPOSITORY_URL = https://github.com/travisrogers05/eap-image-patch
  SOURCE_REPOSITORY_REF = master
  CONTEXT_DIR = ""
  ```

3.  Click on the "Create" button or use this CL command (replace **app-name** with a name you choose):

  ```
  oc process openshift//eap71-basic-s2i APPLICATION_NAME=<app-name> SOURCE_REPOSITORY_URL=https://github.com/travisrogers05/eap-image-patch SOURCE_REPOSITORY_REF=master CONTEXT_DIR="" | oc create -f -
  ```


At this point the contents of the git repository have been added into the container during the build step for the container.  The S2I assemble script for EAP will install all files under the `modules` directory into the container during the build step.  This effectively applies a patched/updated module file to the EAP installation within the container.
