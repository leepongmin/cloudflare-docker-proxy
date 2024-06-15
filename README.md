# cloudflare-docker-proxy

![deploy](https://github.com/ciiiii/cloudflare-docker-proxy/actions/workflows/deploy.yaml/badge.svg)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

> If you're looking for proxy for helm, maybe you can try [cloudflare-helm-proxy](https://github.com/ciiiii/cloudflare-helm-proxy).

## Deploy

1. fork 这个项目
2. 修改Deploy to Cloudflare Workers按钮里面的github仓库为自己仓库的地址
3. 修改 src/index.js 的 const routes 块的内容
   
   ```js
   const routes = {
     "docker.your-domain.com": "https://registry-1.docker.io",
     "quay.your-domain.com": "https://quay.io",
     "registry.your-domain.com": "registry.k8s.io",
   };
   ```
4. 点击deploy to cloudflare workers进行部署

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/leepongmin/cloudflare-docker-proxy)

5. 在 Cloudflare 的 DNS 记录里添加 CNAME 指向部署后的 ${workername}.${username}.workers.dev 地址。
6. 在 Workers 的 HTTP Routes 里，添加 xxx.your-domain.com/* 路由指向 cloudflare-docker-proxy, xxx 就是 docker quay gcr 等

   路由：docker.you-domain.com
   worker：cloudflare-docker-proxy

   前面写了多少镜像地址，就需要写多少条内容，改变前面的路由名称为xxx.you-domain.com即可

7. docker配置加速地址即可
   ```bash
   sudo mkdir -p /etc/docker
   sudo tee /etc/docker/daemon.json <<-EOF
   {
    "registry-mirrors": [
        "https://xxx.you-domain.com"
      ]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

