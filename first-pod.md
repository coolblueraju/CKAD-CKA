Introduction
Pods are the basic unit of work in Kubernetes. Each Pod is comprised of one or more containers that all share the same network space. In this lab step, you will review the basics of Pod configuration. You will work with YAML manifest files that declare Pods, and examine a few fundamental kubectl commands while reviewing the basics of working with Pods. Keep in mind that you should not create standalone Pods. You should normally use a higher-level controller, such as a deployment to gain many benefits, but you will start by reviewing the most basic parts of Pods. 

 

Instructions
1. Load kubectl shell completions for your current shell session:
watch kubectl get nodes
source <(kubectl completion bash)
With completions loaded, you can press tab to autocomplete or list available completions as you enter kubectl commands.

 

2. Issue the following command to create a simple Pod manifest file:

cat << 'EOF' > first-pod.yaml
apiVersion: v1            # The API path for the Pod resource
kind: Pod                 # The kind of resource (Pod)
metadata:
  name: first-pod         # Name of the Pod
spec:
  containers:             # List of containers in the Pod
  - image: httpd:2.4.38   # Container image (using a tag to specify version 2.4.38) 
    name: first-container # Name of the container
EOF
Read through the comments (following #). The manifest is minimal in the sense that all of the fields are required to create a Pod. For simplicity, some fields that you would usually define for an Apache webserver (httpd), such as container port information, are omitted.

 

3. Use the explain command to get an explanation of a Pod's spec (press space to page through the output):

kubectl explain Pod.spec | more


You can always get more details about a resource field by using the explain command. The pattern for the command is

kubectl explain <Resource_Kind>.<Path_To_Field>
where the <Resource_Kind> is the kind of resource and <Path_To_Field> is the path to the field joined by dots (.). For example, if you wanted to get information about the Pod's container's image field, you could enter 

kubectl explain Pod.spec.containers.image
 

4. Create the pod by entering:

kubectl create -f first-pod.yaml


 

5. Get a summary of the Pod resource by entering:

kubectl get pod


This command provides a summary of all Pods in the default namespace. Alternatively, you could get a summary for a specific Pod by providing its name as the last argument, e.g. kubectl get pod first-pod.

The READY column indicates how many containers in the Pod are ready. The STATUS column displays Running when all containers have been created and at least one is still running. You can see this link to learn about the other possible status phases.

 

6. To get all the fields of a resource, you can use the -o or --output option and specify yaml as the output type as in:


kubectl get pod first-pod -o yaml | more
alt

Kubernetes adds many more fields compared to the original manifest file. Many of them help Kubernetes manage the resources such as resourceVersion, selfLink, uid, and status. However, the fields in the spec are available for you to declare in manifest files to configure your Pods. Take a moment to page through the output and see all of the Pod resource fields.

 

7. Delete the Pod:


kubectl delete pod first-pod
You could alternatively specify the -f option to delete the Pod using the file that created it, e.g. kubectl delete -f first-pod.yaml.

 

Summary 
In this lab step, you reviewed the basics of declaring Pods and used a few kubectl commands to work with Pods.
