# Docker Desktop for Windows 與 Azure Kubernetes Service 實機操作

## 刪除 Azure Container Registry 與 Azure Kubernetes Service 叢集之資源群

在命令列模式下達以下指令即會刪除自 Lab 5 至 Lab 8 過程中所建立的 Azure Container Registry 與 Azure Kubernetes Service 叢集，整個過程會花費數分鐘時間，此外由 Azure Kubernetes Service 建立的額外 MC_myResourceGroup_myAKS_southeastasia 也會一併刪除。

```powershell
az group delete --name myResourceGroup
```

* [返回 README](README.md)
