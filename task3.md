<pre>

[student@workstation ~]$ 
[student@workstation ~]$ 
[student@workstation ~]$ 



[student@workstation ~]$ oc adm create-bootstrap-project-template -o yaml &gt; template.yaml
[student@workstation ~]$ vi template.yaml 
[student@workstation ~]$ oc create -f template.yaml -n openshift-config
template.template.openshift.io/project-request created
[student@workstation ~]$ oc get templates -n openshift-config
NAME              DESCRIPTION   PARAMETERS    OBJECTS
project-request                 5 (5 blank)   6
[student@workstation ~]$ oc describe templates -n openshift-config
Name:		project-request
Namespace:	openshift-config
Created:	About a minute ago
Labels:		&lt;none&gt;
Annotations:	&lt;none&gt;

Parameters:	 
    Name:	PROJECT_NAME
    Required:	false
    Value:	&lt;none&gt;

    Name:	PROJECT_DISPLAYNAME
    Required:	false
    Value:	&lt;none&gt;

    Name:	PROJECT_DESCRIPTION
    Required:	false
    Value:	&lt;none&gt;

    Name:	PROJECT_ADMIN_USER
    Required:	false
    Value:	&lt;none&gt;

    Name:	PROJECT_REQUESTING_USER
    Required:	false
    Value:	&lt;none&gt;


Object Labels:	&lt;none&gt;

Message:	&lt;none&gt;

Objects:					 
    Project.project.openshift.io		${PROJECT_NAME}
    RoleBinding.rbac.authorization.k8s.io	admin
    NetworkPolicy.networking.k8s.io		allow-from-openshift-ingress
    NetworkPolicy.networking.k8s.io		allow-same-namespace
    LimitRange					${PROJECT_NAME}-resource-limits
    ResourceQuota				${PROJECT_NAME}-compute-resources
[student@workstation ~]$ oc edit project.config.openshift.io/cluster
project.config.openshift.io/cluster edited
[student@workstation ~]$ watch oc get pods -n openshift-apiserver
[student@workstation ~]$ oc get pods -n openshift-apiserver
NAME                        READY   STATUS    RESTARTS   AGE
apiserver-6f46d9ff8-ht9gp   2/2     Running   0          4m47s
apiserver-6f46d9ff8-rb29m   2/2     Running   0          3m26s
apiserver-6f46d9ff8-vrx8w   2/2     Running   0          2m
[student@workstation ~]$ oc new-project myproject
Error from server (InternalError): Internal error occurred: ResourceQuota &quot;myproject-compute-resources&quot; is invalid: [spec.hard[limit.cpu]: Invalid value: &quot;limit.cpu&quot;: must be a standard resource type or fully qualified, spec.hard[limit.cpu]: Invalid value: &quot;limit.cpu&quot;: must be a standard resource for quota]
[student@workstation ~]$ vi template.yaml 
[student@workstation ~]$ oc replace -f template.yaml -n openshift-config
template.template.openshift.io/project-request replaced
[student@workstation ~]$ watch oc get pods -n openshift-apiserver
[student@workstation ~]$ oc new-project myproject
Error from server (InternalError): Internal error occurred: ResourceQuota &quot;myproject-compute-resources&quot; is invalid: [spec.hard[limit.cpu]: Invalid value: &quot;limit.cpu&quot;: must be a standard resource type or fully qualified, spec.hard[limit.cpu]: Invalid value: &quot;limit.cpu&quot;: must be a standard resource for quota]
[student@workstation ~]$ vi template.yaml 
[student@workstation ~]$ oc replace -f template.yaml -n openshift-config
template.template.openshift.io/project-request replaced
[student@workstation ~]$ oc new-project myproject
Now using project &quot;myproject&quot; on server &quot;https://api.ocp4.example.com:6443&quot;.

You can add applications to this project with the &apos;new-app&apos; command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/serve_hostname

[student@workstation ~]$ oc describe project myproject
Name:		myproject
Created:	48 seconds ago
Labels:		name=myproject
Annotations:	openshift.io/description=
		openshift.io/display-name=
		openshift.io/requester=admin
		openshift.io/sa.scc.mcs=s0:c25,c20
		openshift.io/sa.scc.supplemental-groups=1000640000/10000
		openshift.io/sa.scc.uid-range=1000640000/10000
Display Name:	&lt;none&gt;
Description:	&lt;none&gt;
Status:		Active
Node Selector:	&lt;none&gt;
Quota:
	Name:		myproject-compute-resources
	Resource	Used	Hard
	--------	----	----
	limits.cpu	0	4
	limits.memory	0	4Gi
	pods		0	20
	requests.cpu	0	2
	requests.memory	0	1Gi
Resource limits:
	Name:		myproject-resource-limits
	Type		Resource	Min	Max	Default Request	Default Limit	Max Limit/Request Ratio
	----		--------	---	---	---------------	-------------	-----------------------
	Container	memory		-	2Gi	100Mi		2Gi		-
	Container	cpu		-	2	100m		2		-
[student@workstation ~]$ 
</pre>