hexo-generator
===

This is a simple docker image I used with gitlab-ci to quickly generate a hexo blog and deploy it.

### Example .gitlab-ci.yml

``` yaml
deploy:
    image: zingmars/hexo-generator
    script:
        - npm update
        - hexo generate
        - rsync -av public/ /deployment/ # /deployment is mounted by the runner running this task. You can also use hexo's inbuilt deployment methods.
    cache:
        key: ${CI_COMMIT_REF_SLUG}
        paths:
            - node_modules/
            # you can also cache public/, though you will need to run hexo clean if you change plugins (themes) etc. around.
    tags:
        - blog
    allow_failure: false
```

### Example gitlab-runner config.toml

```
[[runners]]
  name = "blog"
  url = ""
  token = ""
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "zingmars/hexo-generator:latest"
    privileged = false
    disable_cache = false
    volumes = ["/cache", "/var/www/blog:/deployment:rw"]
    shm_size = 0
  [runners.cache]
```
