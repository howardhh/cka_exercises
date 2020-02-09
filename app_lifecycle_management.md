### Understand deployments and how to perform rolling update and rollbacks
1. Create the DaemonSet with nginx:1.10.1 image.
2. Update the DaemonSet above with newer version of the nginx:1.12.1-alpine server.
3. Change the updateStrategy to OnDelete and rollback to revision 1.

###
