## A container approach to enabled personal IPython notebooks in OpenShift (Windows cmd example)


### Assumptions:
* This uses Python 2.7  
* Store workbooks in the “notebooks” folder otherwise documents will dissappear.
* This currently uses my public githib repo; this should be moved internally to complete the effort.
* Heavy processing needs
* There is a single user password associated with each workbook so this is not a long-term enterprise offering.


```
set http_proxy=			#optional  
set http_proxy=			#optional  
set PROJECT=ipynb27           #Change name as appropriate
set IPRPOJECT_PASSWORD		#this will set notebook passcode  
oc new-project tmwpynotebook  #Change name to your own!  
```
