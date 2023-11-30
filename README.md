# Hướng dẫn Cài Đặt Kubernetes Cluster sử dụng Vagrant và VMware

## Yêu Cầu

- [Vagrant](https://www.vagrantup.com/) đã được cài đặt.
- [Vagrant VMware Desktop Plugin](https://www.vagrantup.com/docs/providers/vmware#installation) đã được cài đặt.
- [VMware Desktop](https://www.vmware.com/products/workstation-pro.html) đã được cài đặt và có giấy phép hợp lệ.

## Bước 1: Cài Đặt Plugin VMware

Chạy lệnh sau để cài đặt plugin VMware cho Vagrant:

```bash
vagrant plugin install vagrant-vmware-desktop
```

## Bước 2: Triển Khai Kubernetes Cluster

 ``` bash 
 vagrant up --provider=vmware_desktop 
 ```

## Bước 3: Kết Nối vào Master Node

 ```bash 
 vagrant ssh master
 ```

 ## Bước 4: Khởi Tạo Kubernetes Cluster

``` bash 
sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

Thêm dòng này vào cuối file : 

```bash 
Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"
```

run command : 

```bash 
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 
```

 ## Bước 5: Thêm Worker Nodes vào Cluster

 Truy Cập vào các worker node : 

``` bash 
sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

Thêm dòng này vào cuối file : 

```bash 
Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"
```

Sử dụng lệnh kubeadm join được hiển thị sau khi khởi tạo cluster ở Bước 4 để thêm worker nodes vào cluster. Chạy các lệnh này trên mỗi worker node:

```bash 
sudo kubeadm join <master-ip>:<master-port> --token <token> --discovery-token-ca-cert-hash <hash>
```


 ## Bước 6: Kết Nối kubectl Configuration

 Trở về Master Node : 
 ```bash 
 mkdir -p $HOME/.kube

 sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

 sudo chown $(id -u):$(id -g) $HOME/.kube/config
 ```

## Bước 7: Cài Đặt CNI (Container Network Interface)
```bash 
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.4/manifests/tigera-operator.yaml

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.4/manifests/custom-resources.yaml
```

## Bước 8: Kiểm Tra Cluster Status

```bash 
kubectl get nodes
```
