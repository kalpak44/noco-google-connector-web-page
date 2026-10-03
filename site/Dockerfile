# ── validate stage: reject malformed markup before it can be served ──────────
# runtime copies the pages from here rather than from the build context, which
# is what forces this stage to run — BuildKit skips a stage nothing depends on.
FROM node:25-alpine AS validate
WORKDIR /src
COPY .htmlvalidate.json ./
COPY public/ ./public/
RUN npx --yes html-validate "public/*.html"

# ── runtime stage ─────────────────────────────────────────────────────────────
FROM nginx:1.31-alpine AS runtime

ARG BUILD_DATE
ARG VCS_REF
ARG VERSION

LABEL org.opencontainers.image.title="noco-google-connector-web-page" \
      org.opencontainers.image.description="Landing page for Google Connector NocoBase plugin" \
      org.opencontainers.image.url="https://noco-google-connector.pavel-usanli.online/" \
      org.opencontainers.image.source="https://github.com/kalpak44/noco-google-connector-web-page" \
      org.opencontainers.image.licenses="MIT" \
      org.opencontainers.image.created="${BUILD_DATE}" \
      org.opencontainers.image.revision="${VCS_REF}" \
      org.opencontainers.image.version="${VERSION}"

# Remove default config; install wget for health check
RUN rm /etc/nginx/conf.d/default.conf

# The pinned nginx base image still ships four packages that alpine 3.24 has
# already fixed. Upgrade just those packages so the vulnerabilities do not ship
# while we wait for a rebuilt base image:
#   CVE-2026-93990  libexpat    2.8.4-r0  -> 2.8.5-r0
#   CVE-2026-46675  libpng      1.6.58-r1 -> 1.6.59-r0
#   CVE-2026-103111 pcre2       10.48-r0  -> 10.49-r0
#   CVE-2026-58055  nghttp2-libs 1.69.0-r0 -> 1.70.0-r0
RUN apk upgrade --no-cache libexpat libpng pcre2 nghttp2-libs

# Custom nginx config
COPY conf/nginx.conf /etc/nginx/conf.d/default.conf

# Static site files. robots.txt and sitemap.xml are served too: robots.txt
# advertises the sitemap URL, so shipping one without the other points
# crawlers at a 404.
COPY --from=validate --chown=nginx:nginx /src/public/ /usr/share/nginx/html/

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
    CMD wget -qO /dev/null http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
