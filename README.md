# noco-ai-tools — website

Static landing page, privacy policy and terms of service for the **Noco AI Tools**
NocoBase plugin, served at <https://noco-ai-tools.pavel-usanli.online/>.

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
docker build -t noco-tools-site site
docker run --rm -p 8080:80 noco-tools-site
# http://localhost:8080
```

## Deploy

`.github/workflows/site.yml` builds `site/` on every push to `main` that touches
it and pushes the image to `ghcr.io/kalpak44/noco-tools-site`, tagged with the
short commit SHA and `latest`.

## License

MIT
