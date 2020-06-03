# Vault

HashiCorp Vault server & Backend management UI

Backend: http://127.0.0.1:8200/ui

## Usage

```shell
$ docker-compose up -d
```

```shell
export VAULT_ADDR='http://127.0.0.1:8200'
```

```shell
$ vault operator unseal <key> #See key in json file
```

## How setting up vault Server?

```shell
$ vault operator init -key-shares=6 -key-threshold=3
```

### Unseal  

We need to supply it with 3 key shares:

```shell
$ vault operator unseal <key 1>
$ vault operator unseal <key 2>
$ vault operator unseal <key 3>
```

### Seal status

```shell
$ vault status -format=json
```

### Login as root

```shell
$ vault login <token>
```

### Enable path secrets
```shell
$ vault secrets enable -version=1 -path=v1 kv
```

### Enable userpass auth

```shell
$ vault auth enable userpass
```

### Enable approle auth

```shell
$ vault auth enable approle
```

### Enable kv engine

```shell
$ vault secrets enable -version=1 -path=v1 kv
```

### Add policies
[From Vault Backend](http://127.0.0.1:8200/ui)

- Login as root user.
- Go to `http://127.0.0.1:8200/ui/vault/policies/acl/create` and select `upload file`.

### Add userpass user

```shell
$ vault write auth/userpass/users/authorizer policies=admin-vault password=helloworld
```

### Config approles

```shell
$ vault login -method=userpass username=authorizer password=helloworld

$ vault auth enable approle # if required

$ vault write auth/approle/role/admin secret_id_ttl=10m token_num_uses=10 token_ttl=20m token_max_ttl=30m secret_id_num_uses=40 policies=admin

$ vault write auth/approle/role/insdes secret_id_ttl=10m token_num_uses=10 token_ttl=20m token_max_ttl=30m secret_id_num_uses=40 policies=insdes

$ vault write auth/approle/role/insdessr secret_id_ttl=10m token_num_uses=10 token_ttl=20m token_max_ttl=30m secret_id_num_uses=40 policies=insdessr

$ vault write auth/approle/role/qa secret_id_ttl=10m token_num_uses=10 token_ttl=20m token_max_ttl=30m secret_id_num_uses=40 policies=qa

$ vault write auth/approle/role/student secret_id_ttl=10m token_num_uses=10 token_ttl=20m token_max_ttl=30m secret_id_num_uses=40 policies=student
```
