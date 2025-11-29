# Break-Games vending machine
Break-Games Vending Machine is a webapp which, using basic HTML + JS, implements a digital vending machine with simple multiplayer games. The user selects player quantity, break time length and , the vending machine prints a QR code with a URL to connect to and, through Kubernetes a container is automatically deployed, the container has a limited lifetime (just enough to play some games during breaktime ~10 minutes) after that it will be killed and game statistics saved into a external database.



## Issues notes
### ParrotOS repository conflict
ParrotOS version 6.4 has codename `lory` which is not available with recommended Docker installation, had to manually swap to `trixie`.

### Docker deamon and minikube deamon
Docker deamon changed to minikube one with ```eval $(minikube docker-env)```, else the images were saving in default docker deamon and kubernetes couldnt find them.

### Service type
Using `LoadBalancer` type instead of `NodePort` in for services is not supported by `minikube` cluster.

### Minikube no external IP
Command `minikube service --all` returns:
```
❌  Exiting due to MK_UNIMPLEMENTED: minikube service is not currently implemented with the builtin network on QEMU
```
