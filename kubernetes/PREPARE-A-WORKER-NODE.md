# prepare a worker node

Work all of the preparation up to the point of installing a cluster w/ kubeadm in PREPARE-AND-BOOTSTRAP-CONTROL-PLANE-NODE. Then use the kubeadm join output from turning up the cluster. You will see output like the following

    [preflight] Running pre-flight checks
	[WARNING SystemVerification]: missing optional cgroups: hugetlb
    [preflight] Reading configuration from the "kubeadm-config" ConfigMap in namespace "kube-system"...
    [preflight] Use 'kubeadm init phase upload-config kubeadm --config your-config-file' to re-upload it.
    [kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/instance-config.yaml"
    [patches] Applied patch of type "application/strategic-merge-patch+json" to target "kubeletconfiguration"
    [kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
    [kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
    [kubelet-start] Starting the kubelet
    [kubelet-check] Waiting for a healthy kubelet at http://127.0.0.1:10248/healthz. This can take up to 4m0s
    [kubelet-check] The kubelet is healthy after 1.502660925s
    [kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap

    This node has joined the cluster:
    * Certificate signing request was sent to apiserver and a response was received.
    * The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

    kubectl get nodes
    NAME      STATUS   ROLES           AGE    VERSION
    albus     Ready    control-plane   166m   v1.34.1
    gandalf   Ready    <none>          136m   v1.34.1

