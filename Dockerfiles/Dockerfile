FROM alpine:3.16 as builder

RUN set -eux \
	&& apk add --no-cache \
		bash \
		curl

ARG VERSION=latest
RUN set -eux \
	&& if [ "${VERSION}" = "latest" ]; then \
		VERSION="$( curl -Ss https://github.com/cytopia/linkcheck/tags \
			| tac \
			| tac \
			| grep -Eo 'cytopia/linkcheck/releases/tag/v[.0-9]+' \
			| grep -Eo '[.0-9]+[0-9]' \
			| sort -V \
			| tail -1 )"; \
	fi \
	&& curl -sS https://raw.githubusercontent.com/cytopia/linkcheck/v${VERSION}/linkcheck > /usr/bin/linkcheck \
	&& chmod +x /usr/bin/linkcheck \
	\
	&& linkcheck --version


FROM alpine:3.16 as production
ARG VERSION=latest
# https://github.com/opencontainers/image-spec/blob/master/annotations.md
#LABEL "org.opencontainers.image.created"=""
#LABEL "org.opencontainers.image.version"=""
#LABEL "org.opencontainers.image.revision"=""
LABEL "maintainer"="cytopia <cytopia@everythingcli.org>"
LABEL "org.opencontainers.image.authors"="cytopia <cytopia@everythingcli.org>"
LABEL "org.opencontainers.image.vendor"="cytopia"
LABEL "org.opencontainers.image.licenses"="MIT"
LABEL "org.opencontainers.image.url"="https://github.com/cytopia/docker-linkcheck"
LABEL "org.opencontainers.image.documentation"="https://github.com/cytopia/docker-linkcheck"
LABEL "org.opencontainers.image.source"="https://github.com/cytopia/docker-linkcheck"
LABEL "org.opencontainers.image.ref.name"="linkcheck ${VERSION}"
LABEL "org.opencontainers.image.title"="linkcheck ${VERSION}"
LABEL "org.opencontainers.image.description"="linkcheck ${VERSION}"

RUN set -eux \
	&& apk add --no-cache \
		bash \
		curl

COPY --from=builder /usr/bin/linkcheck /usr/bin/linkcheck
WORKDIR /data
ENTRYPOINT ["linkcheck"]
CMD ["--version"]
