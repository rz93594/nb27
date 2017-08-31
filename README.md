## A container approach to enabled personal IPython notebooks in OpenShift (Windows cmd example)


### Assumptions:
* This uses Python 2.7  
* Store workbooks in the “notebooks” folder otherwise documents will dissappear.
* This currently uses my public githib repo; this should be moved internally to complete the effort.
* Unsure about Heavy processing needs (haven't pushed the CPU constraints of openshift)
* There is a single user password associated with each workbook so this is not a long-term enterprise offering.


```
set http_proxy=			#optional  
set http_proxy=			#optional  
set PROJECT=ipynb27             #Change name as appropriate
set IPRPOJECT_PASSWORD		#this will set notebook passcode  
oc new-project tmwpynotebook    #Change name to your own!  
```


### Create python application:

```
oc new-app --name=%PROJECT% -e APP_SCRIPT="run.sh" ^
--build-env HTTPS_PROXY="40.0.40.10:9000" ^
--build-env HTTP_PROXY="40.0.40.10:9000" ^
--build-env UPGRADE_PIP_TO_LATEST=true ^
--build-env JUPYTER_NOTEBOOK_PASSWORD=%IPROJECT_PASSWORD% python:2.7~https://github.com/rz93594/nb27.git
```


### Watch for the project to be deployed and finish before moving to next step:

```
oc logs -f bc/%PROJECT%
```


### Add the permanence storage for notebooks:
NOTE:  If using in teams, work with the OpenShift team to mount up a shared repository from outside of OpenShift (like lrlhpc.  

```
oc volume dc/%PROJECT% --add --mount-path=/opt/app-root/src/notebooks --claim-mode=rwo --claim-size=512Mi -t pvc
```

### Create the browser URL’s and enable SSL:

```
oc expose service %PROJECT%

oc patch route/%PROJECT% -p "{\"spec\":{\"tls\":{\"termination\":\"edge\",\"insecureEdgeTerminationPolicy\":\"Redirect\"}}}"

oc get routes
```

### You are done!

You may now login into the URL provided from the oc get routes command, in this example it would be:

https://ipynb27-tmwpynotebook.paas-poc.am.lilly.com

If you need to add additional packages, you can add them via !pip install packagename  however the preffered path would be to add the needed package to "requirements.txt" in the github repo  and redepoy the container.

### To delete the project: (WARNING!  This will make your data dissapear!)

```
oc delete project tmwpynotebook
```



### ToDo

* Move repo into the company org and add access instructions 
* Mount external shared NAS into notebook path
* Pass uid|gid variables to container for data access
