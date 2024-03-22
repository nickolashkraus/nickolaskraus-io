<div align="center">
  <a href="https://nickolaskraus.io">
    <p>NickolasKraus.io</p>
  </a>
  <p align="center">My personal website</p>
</div>

---

## Development

Initialize and update submodules:

```bash
git submodule init
git submodule update
```

### Git Submodules

The `git submodule update` command fetches the commit specified in `.git/modules` for each submodule of the parent Git repository. In order to update a submodule to the latest commit available from its remote reference, you will need to pull it directly:

```bash
cd <path/to/submodule>
git pull origin master
cd <path/to/parent>
git add . && git commit -m "Update submodules"
```

## Deploy

```bash
cd tf/
terraform init
terraform plan
terraform apply
```

**NOTE**: The Amazon CloudFront distribution can take up to 30 minutes to provision.

## Publish Content

```bash
./publish
```
