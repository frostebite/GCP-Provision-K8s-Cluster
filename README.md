Use this action to use Kubernetes to provision a K8s cluster. 

# Provisioning
If a cluster with the same name already exists it will be used, if no cluster with the same name is found one will be created.

## Cloud provider support
This action supports only GCP.

# Input
A lockTimeout can be set to cleanup the cluster incase a lock isn't cleaned up. The default is set to 360, this means after 360 minutes the lock will expire and the cluster will be cleaned up.

A shutdownPollingFrequency can be set to control how often the cluster will check if no locks remain.

A clusterCooldownPeriod can be set to prevent the cluster from being deleted for a certain number of minutes after the last lock has been removed.

# Output
The action will return a base64 Kubernetes config which can be used to access the cluster.
The action will also return the a lock key, this lock key is then used when disposing the cluster.

# Cleaning up the cluster
At the end of running work on the cluster, the [Dispose-K8s-Cluster]() action should be used to destroy the cluster. The dispose action will release the lock that prevents shutdown and then check if any other locks remain, if any do the cluster will not be deleted, if no locks remain the cluster is not running any work and will be shutdown.

## Locks
Locks are used in this action to prevent the cluster being cleaned up while other workloads may still be running. Namespaces are currently used to lock a cluster.
