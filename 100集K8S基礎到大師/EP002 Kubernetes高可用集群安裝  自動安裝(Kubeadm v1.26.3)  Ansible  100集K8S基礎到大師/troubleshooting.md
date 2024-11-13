我依序把roles檔案給您，先給common，還有一個sysctl-k8s.conf.j2稍候補上

我依序把roles檔案給您，再來是docker，還有一個sysctl-k8s.conf.j2稍候補上

我依序把roles檔案給您，再來是k8s-base

我依序把roles檔案給您，再來是haproxy

我依序把roles檔案給您，再來是keepalived


我依序把roles檔案給您，再來是k8s-master

我依序把roles檔案給您，再來是k8s-node

我依序把roles檔案給您，再來是calico-network

我依序把roles檔案給您，再來是k8s-completion

我依序把roles檔案給您，再來是cert-manager


我依序把roles檔案給您，再來是cluster-validation



```bash
執行流程會報錯，如最下方報錯訊息，我判斷可能的原因如下兩項

1. 我要下載和安裝的指令如下，但是在inventory/group_vars/all.yml裡面寫的cri_dockerd_version: 0.3.3.3-0，在roles/k8s-base/tasks/main.yml裡面的Download cri-dockerd deb package，會變成下載
https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3.3-0/cri-dockerd_0.3.3.3-0.jammy_x86_64.deb跟我要的不一樣，所以不能下載，再幫我修正

2. 同時{{ ansible_distribution_release }}_{{ ansible_architecture }}.deb解析出來是jammy_x86_64.deb，我要的是ubuntu-jammy_amd64.deb才對


我要下載和安裝的指令：
root@k8s-master71:~# wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3/cri-dockerd_0.3.3.3-0.ubuntu-jammy_amd64.deb

root@k8s-master71:~# sudo dpkg -i cri-dockerd_0.3.3.3-0.ubuntu-jammy_amd64.deb 


inventory/group_vars/all.yml文件中給的變數值：
cri_dockerd_version: 0.3.3.3-0


roles/k8s-base/tasks/main.yml文件中的流程：
- name: Download cri-dockerd deb package
  get_url:
    url: "https://github.com/Mirantis/cri-dockerd/releases/download/v{{ cri_dockerd_version }}/cri-dockerd_{{ cri_dockerd_version }}.{{ ansible_distribution_release }}_{{ ansible_architecture }}.deb"
    dest: "/tmp/cri-dockerd.deb"


報錯訊息：
TASK [k8s-base : Download cri-dockerd deb package] *******************************************************************************************************************
fatal: [{{ worker1_hostname }}]: FAILED! => {"changed": false, "dest": "/tmp/cri-dockerd.deb", "elapsed": 0, "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3.3-0/cri-dockerd_0.3.3.3-0.jammy_x86_64.deb"}
fatal: [{{ master2_hostname }}]: FAILED! => {"changed": false, "dest": "/tmp/cri-dockerd.deb", "elapsed": 0, "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3.3-0/cri-dockerd_0.3.3.3-0.jammy_x86_64.deb"}
fatal: [{{ worker2_hostname }}]: FAILED! => {"changed": false, "dest": "/tmp/cri-dockerd.deb", "elapsed": 0, "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3.3-0/cri-dockerd_0.3.3.3-0.jammy_x86_64.deb"}
fatal: [{{ master3_hostname }}]: FAILED! => {"changed": false, "dest": "/tmp/cri-dockerd.deb", "elapsed": 0, "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3.3-0/cri-dockerd_0.3.3.3-0.jammy_x86_64.deb"}
fatal: [{{ master1_hostname }}]: FAILED! => {"changed": false, "dest": "/tmp/cri-dockerd.deb", "elapsed": 0, "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.3.3-0/cri-dockerd_0.3.3.3-0.jammy_x86_64.deb"}
```