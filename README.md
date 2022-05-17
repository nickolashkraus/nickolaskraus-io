# [NickolasKraus.io](https://nickolaskraus.io)

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/NickolasHKraus/nickolaskraus-io/blob/master/LICENSE)

[NickolasKraus.io](https://nickolaskraus.io) is my personal website. It is generated using [Hugo](https://gohugo.io) and hosted on [AWS](https://aws.amazon.com).

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
terraform init
terraform plan
terraform deploy
```

**NOTE**: The Amazon CloudFront distribution can take up to 30 minutes to provision.

## Publish Content

```bash
./publish
```
