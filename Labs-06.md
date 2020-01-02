# Docker Desktop for Windows 與 Azure Kubernetes Service 實機操作

## Lab 6 建立 Azure Kubernetes Service 叢集

1.此實作練習會使用 [Lab 5](Labs-05.md) 所建立的 myResourceGroup 與已建立的 Azure Container Registry (ACR)，請紀錄之前建立的 ACR 名稱。在命令列模式下，如下鍵入指令以確認目前環境已經安裝妥了 Azure CLI 2.0.77 之後版本，若版本太舊或沒有安裝 Azure CLI 先[下載](https://docs.microsoft.com/zh-tw/cli/azure/install-azure-cli?view=azure-cli-latest) 並安裝妥 Azure CLI。 

```powershell
az --version
```

2.接著如下建入指令建立一個擁有三個 Standard_B2s 規格的節點，名為 myAKS 的 Azure Kubernetes Services (AKS) 叢集，並且具備權限可以存取之前所建立的 Azure Container Registry (ACR)，建立 Azure Kubernete Service 叢集會花費數分鐘時間，建立完成後會產生一個名為 MC_myResourceGroup_myAKS_southeastasia 的資源群組，將相關節點與虛擬網路置放在此新建的資源群組之中

```powershell
az aks create `
    -g myResourceGroup `
    -n myAKS `
    --node-count 3 `
    --node-vm-size Standard_B2s `
    --generate-ssh-keys `
    --attach-acr <ACR 名稱>
    
```

3.所有操作將使用 Kubernetes 命令列用戶端工具 **kubectl**，安裝 Docker Desktop for Windows 已經提供此一工具，但若想取得最新版本請在命列下達如下指令安裝 **kubectl**，並將 PATH 環境變數中 $HOME\.azure-kubectl 路徑搜尋順序優先於 Docker 安裝路徑
```powershell 
az aks install-cli
```

4.在命令列模式下達以下指令將下載 Kubernetes 憑證至 Windows 環境以供 Kubernetes 命令列用戶端工具使用，這些資訊會儲存在 $HOME\.kube 之內

```powershell
az aks get-credentials -g myResourceGroup -n myAKS
```

如果您的電腦中已經有使用其他的 Kubernetes 叢集，可以鍵入以下指令切換到此一新建立的 Kubernetes 叢集，如果沒有其他的叢集可以忽略此步驟

```powershell
kubectl config use-context myAKS
```

5.在命令列模式下達以下指令，列出所建立的 Auzre Kubernetes Service (AKS) 叢集內所有節點，確認環境已經正常運作
```powershell 
kubectl get node
```
應會顯示類似如下結果 :

| NAME                              | STATUS | ROLES | AGE | VERSION |
|-----------------------------------|--------|-------|-----|---------|
| aks-nodepool1-32289653-vmss000000 | Ready  | agent | 12m | v1.14.8 |
| aks-nodepool1-32289653-vmss000001 | Ready  | agent | 12m | v1.14.8 |
| aks-nodepool1-32289653-vmss000002 | Ready  | agent | 12m | v1.14.8 |

6.在命令列模式下達以下指令，將叢集內所有節點數量縮限為兩個節點，請注意 Auzre Kubernetes Service (AKS) 叢集至少需要兩個節點才能運行，當節點數量不足可能造成應用程式無法順利部署，此命令需要花費數分鐘時間完成。
```powershell 
az aks scale -g myResourceGroup -n myAKS -c 2
```

待指令完成後下達以下指令:
```powershell 
kubectl get node
```
應會顯示類似如下結果 :

| NAME                              | STATUS | ROLES | AGE | VERSION |
|-----------------------------------|--------|-------|-----|---------|
| aks-nodepool1-32289653-vmss000000 | Ready  | agent | 18m | v1.14.8 |
| aks-nodepool1-32289653-vmss000001 | Ready  | agent | 18m | v1.14.8 |
| aks-nodepool1-32289653-vmss000002 | NotReady,SchedulingDisabled  | agent | 12m | v1.14.8 |

了解如何設定 AKS 節點數量方式後，下達以下指令恢復為三個節點。

```powershell 
az aks scale -g myResourceGroup -n myAKS -c 3
```
應會顯示類似如下結果 :

| NAME                              | STATUS | ROLES | AGE | VERSION |
|-----------------------------------|--------|-------|-----|---------|
| aks-nodepool1-32289653-vmss000000 | Ready  | agent | 19m | v1.14.8 |
| aks-nodepool1-32289653-vmss000001 | Ready  | agent | 19m | v1.14.8 |
| aks-nodepool1-32289653-vmss000002 | Ready  | agent | 4m | v1.14.8 |

請暫時保留本 Lab 所建立之環境以供後續 Lab 使用。

* [前往練習 Lab 7](Labs-07.md)
* [返回 README](README.md)
