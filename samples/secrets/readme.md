#### Using a StorageClass, PersistentVolume, PersistentVolumeClaim, and ConfigMap

This example shows different types of storage options that can be used. Because a local Kubernetes is running (with a single Node), we'll
only use the local-storage option, however in cloud scenarios the SC/PVC could be modififed as appropriate for the cloud provider's storage options.

## Running the MongoDB Deployment

1. Create the following folder structure on your local system:

    Mac/Linux: `/tmp/data/db`
    Windows:   `c:/temp/data/db`

    If you’re running Docker Desktop Kubernetes on **Windows**, update the `PersistentVolume` definition:

    - **Native Windows backend (Hyper-V/VM):**  
    Set the `hostPath` path to `/run/desktop/mnt/host/c/temp/data/db`.

    - **WSL2 backend:**  
        Set the `hostPath` path to `/mnt/c/temp/data/db`

3. Run the following to add the database passwords as secrets:

    `kubectl create secret generic db-passwords --from-literal=db-password='password' --from-literal=db-root-password='password'`

3. Start up the Pod:

    `kubectl create -f mongo.deployment.yml`

4. Run `kubectl get pods` to see the pod.
5. Run `k exec [mongo-pod-name] -it -- sh` to shell into the container. Run the `mongosh` command to make sure the database is working. Type `exit` to exit the shell.

    Note: If you have a tool that can hit MongoDB externally you can `kubectl port-forward` to the pod to expose 27017.

6. Delete the mongo Pod: `kubectl delete pod [mongo-pod-name]`
7. Once the pod is deleted, run `kubectl get pv` and note the reclaim policy that's shown and the status (should show Bound since the policy was Retain
8. Delete everything else: `kubectl delete -f mongo.deployment.yml`