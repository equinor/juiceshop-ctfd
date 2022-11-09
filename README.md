# juiceshop-ctfd
A set of scripts and instructions for hosting CTF based on juiceshop in CTF mode and
using CTFd to track progress


# Setting up the service
Follow instructions on [multijuicer site](https://github.com/iteratec/multi-juicer/blob/main/guides/openshift/openshift.md)
but with the following modifications

## Step 2
For this to work we need to allow specifying a non-root user to be the container runner. Log in as cluster administrator
and run
```bash
oc project multi-juicer
oc adm policy add-scc-to-user nonroot -z default --as system:admin
```

We need to override some values to make it work, override like this
```
helm upgrade --install multi-juicer multi-juicer/multi-juicer --values values.yaml
```

# Setting up CTFD with MariaDB

```bash
oc new-app -e MYSQL_USER=<your_value_here> -e MYSQL_PASSWORD=<your_value_here> -e MYSQL_DATABASE=<your_value_here> registry.redhat.io/rhel8/mariadb-105 --name=mariadb
oc create secret generic maria-connection --from-literal=DATABASE_URL=mysql+pymysql://<MYSQL_USER>:<MYSQL_PASSWORD>@mariadb:3306/<MYSQL_DATABASE>
oc apply -f ctfd.yaml
```

# Configuring CTFD with Juiceshop Challenges
Follow the instructions on [Juice Shop CTF](https://github.com/juice-shop/juice-shop-ctf) site to import juiceshop challenges, hints and flags into the CTFD instance.

Happy Hacking !!