# OpenEBS Rancher w/ NFS Demo

## What
This is a demo deployment OpenEBS on Rancher with NFS.

## How
1. Run `terraform apply` on openebs-rancher-quickstart repo
   1. https://github.com/DoriftoShoes/openebs-rancher-quickstart
2. Edit cluster config to add extra_binds per Rancher OpenEBS guide
   1. https://mayadata.io/assets/pdf/rancher-openebs-solution-docs.pdf
3. Install OpenEBS through apps marketplace in Rancher UI
   1. Disable cStor sparse pools
4. StoragePoolClaim
    1. Get disk ids
       1. `kubectl get disks`
    2. Add disks to "diskList" in nfs-test-spc.yaml
    3. Create SPC
       1. `kubectl apply -f nfs-test-spc.yaml`
5. Connect to MayaOnline
6. Add StorageClass
   1. `kubectl apply -f nfs-test-sc.yaml`
7. Create new namespace
   1. `kubectl create ns nfs-test`
8. Setup helm: https://rancher.com/docs/rancher/v2.x/en/installation/ha/helm-init/
9.  Install nfs-provisioner
    1.  `helm install stable/nfs-server-provisioner --namespace=nfs-test --name=nfs-provisioner --set=persistence.enabled=true,persistence.storageClass=nfs-test,persistence.size=5Gi,storageClass.name=nfs-provisioner-sc`
10. Deploy Jira App
    1.  `kubectl apply -f jira.yaml`
