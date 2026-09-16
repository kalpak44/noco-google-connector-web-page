# noco-google-connector-web-page

Static landing page, privacy policy and terms of service for the **Google Connector**
NocoBase plugin, served at <https://noco-google-connector.pavel-usanli.online/>.

The plugin itself has moved to
[kalpak44/deepcraft-nocobase](https://github.com/kalpak44/deepcraft-nocobase/tree/main/plugins/plugin-noco-google-connector)
(`plugins/plugin-noco-google-connector`). This repository now contains only the
website — issues and pull requests about plugin code belong in the new repo.

## Contents

```
site/
  index.html      landing page
  privacy.html    privacy policy   (required for Google OAuth verification)
  tos.html        terms of service (required for Google OAuth verification)
  robots.txt
  sitemap.xml
  nginx.conf      nginx server config
  Dockerfile      nginx:alpine image serving the pages
```

## Local preview

```bash
docker build -t noco-google-connector-web-page site
docker run --rm -p 8080:80 noco-google-connector-web-page
# http://localhost:8080
```

## Deploy

`.github/workflows/publish.yml` builds `site/` on every pull request and every push
to `main` that touches it. A push publishes `ghcr.io/kalpak44/noco-google-connector-web-page`,
tagged with the short commit SHA and `latest`, then triggers the cluster deploy; a pull
request builds the image but publishes nothing.

## License

MIT
