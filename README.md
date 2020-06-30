## Local deployment

Assuming docker is already installed

```
echo "127.0.0.1 openresty" >> /etc/hosts
export PORT=8080
cd web
docker build --tag openresty:1.0 .
docker run --publish 8000:8080 --detach --name openresty openresty:1.0
```


## Github actions workflow

https://github.com/bric3/bric3.github.io/tree/hugo-sources/.github/workflows

```
name: GitHub Pages

on:
  push:
    branches:
      - hugo-sources

jobs:
  build-deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
          fetch-depth: 0

      - name: Build
        run: docker run --rm --volume $PWD:/src bric3/hugo-builder hugo --verbose --buildDrafts --buildFuture

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_branch: master
          publish_dir: ./public
          cname: blog.arkey.fr
#          force_orphan: true
          
```