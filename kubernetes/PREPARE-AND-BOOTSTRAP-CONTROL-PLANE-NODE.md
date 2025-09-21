# prep and bootstrap

## prepare

Example is from Debian 12 on a Rasberry Pi 4

Want these two modules to load automatically( not Pi specific).  Needed for containerd

    cat <<EOF | tee /etc/modules-load.d/k8s.conf
    overlay
    br_netfilter
    EOF

Manually load the modules (not Pi specific)

    modprobe overlay
    modprobe br_netfilter

Set the following kernel parameters (not Pi specific)

    cat <<EOF | tee /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-iptables = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    net.ipv4.ip_forward = 1
    EOF

    sysctl --system

Turn off swap (Pi specific to turn off swap)

    dphys-swapfile swapoff
    dphys-swapfile uninstall
    systemctl disable dphys-swapfile

## Install containerd

    apt -y install containerd

Configure containerd to work w/ kubernetes

    containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1

Edit /etc/containerd/config.toml. Under [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options] set

    SystemdCgroup = true

Restart containerd

    systemctl restart containerd

## Add the kubernetes apt repository

    echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.34/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    apt-get update

## Install Kubernetes tools

    apt-get install -y kubelet kubeadm kubectl

Add the following to /boot/firmware/cmdline.txt (turning features on in kernel)

    cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1 swapaccount=1 cgroup_enable=hugetlb

Reboot system. Afterward, check for mounted cgroups. If CONFIG_CGROUP_HUGETLB isn't available, it's not a deal breaker


## Install kubernetes cluster with kubeadm

    IPADDR=$(ip route get 8.8.8.8 | sed -n 's/.\*src \([^\ ]*\).*/\1/p')
    NODENAME=$(hostname -s)
    POD_CIDR=10.10.0.0/16

    kubeadm init --apiserver-advertise-address=$IPADDR  \
                 --apiserver-cert-extra-sans=$IPADDR  \
		 --pod-network-cidr=$POD_CIDR  \
		 --node-name $NODENAME  \
		 --ignore-preflight-errors Swap

The output from the kubeadm init command above will produce this at bottom. Retain this.

    kubeadm join 192.168.245.61:6443 --token <TOKEN> \
	--discovery-token-ca-cert-hash sha256:<SHA256_HASH>

Check nodes. Until you apply a pod network, you will see something like this

    kubectl get nodes
    NAME      STATUS     ROLES           AGE   VERSION
    albus     NotReady   control-plane   20m   v1.34.1

Deploy a pod network to the cluster (pick one to suit your needs https://kubernetes.io/docs/concepts/cluster-administration/addons/)

    kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

After applying the pod network, it will come up

    kubectl get nodes
    NAME      STATUS     ROLES           AGE   VERSION
    albus     Ready      control-plane   25m   v1.34.1

## Turn up a worker node

Work all of the preparation up to the point of installing a cluster w/ kubeadm. Then use the kubeadm join output from turning up the cluster. You will see output like the following

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

