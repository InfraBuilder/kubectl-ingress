# kubectl-ingress
Kubectl plugin for ingress management for linux/mac

# Requirement

You need `kubectl` and `bash`

# Installation

This plugin is made of a simple bash script : [kubectl-ingress](kubectl-ingress) . To install this plugin, it just have to be available in your PATH.

One line installation :
```bash
curl https://kubectl-ingress.infrabuilder.com/kubectl-ingress | sudo tee /usr/local/bin/kubectl-ingress && sudo chmod +x kubectl-ingress
```

# General usage

To list ingresses :
```bash
kubectl ingress list
```

To edit an ingress :
```bash
kubectl ingress edit {name}
```

To create an ingress :
```bash
kubectl ingress create --help
```

To create or update an ingress :
```bash
kubectl ingress apply --help
```

# Standard kubectl parameters

All arguments taht are non specific to this plugin are given to kubectl
command so that you can use `--namespace`, `--context`, `--dry-run`, `-o yaml`, 
and many more as you will do with any other command

You can generate an ingress yaml without sending it on the api :
```bash
kubectl ingress create web --url=example.com=web:80 --dry-run -o yaml > myingress.yml
```

# Create vs Apply

If you want to create an ingress, and do nothing it it already exists, you have to use `create` subcommand.

Example :
```bash
kubectl ingress create web --url=example.com=web:80
```

If you want to create an ingress or update it if it already exist, you have to /update (upsert) 
Example :
```bash
kubectl ingress apply web --url=example.com=web:80
```


# Create/apply usage

Create and apply usage is exactly the same, just the behavior changes.
Following usage is for `create`, and is the output of `kubectl ingress create -h`.

Usage: `kubectl ingress create [opts] {name} --url=hostname/uri=service:port [--url=...]`

Option list :
-  `-a`, `--annotate=key=value`
-  `-ci`, `--cluster-issuer=name` Will enable tls, and tell cert-manager cluster issuer "name" to generate certificates
-  `-h`, `--help` Display this message
-  `-i`, `--issuer=name` Will enable tls, and tell cert-manager issuer "name" to generate certificates
-  `-t`, `--tls` Will activate tls using a secret named "{hostname}-tls" for each hostname
-  `-u`, `--url=hostname/path=service:port` Path is optionnal

# Examples:

Create an ingress rule "web" routing "example.com" to service "web" on port 80
```bash
kubectl ingress create web --url=example.com=web:80
```

Create same ingress with short flags :
```bash
kubectl ingress create web -u example.com=web:80
```

TLS activation :
```bash
kubectl ingress create web-tls -u example.com=web:80 --tls
```

Ingress rule "sites" with multiple url rules and uri routing:
```bash
kubectl ingress create sites -u example.com=web:80 -u api.example.com/api/v1=myapp:8080
```

With custom annotation:
```bash
kubectl ingress create example -u example.com=web:80 -a nginx.ingress.kubernetes.io/rewrite-target=/
```

Using cert-manager (automatically enable --tls):
```bash
kubectl ingress create mysite -u example.com=web:80 --cluster-issuer=letsencrypt
```
