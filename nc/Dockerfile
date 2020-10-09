FROM alpine:3.12
# install the PHP extensions we need
# see https://docs.nextcloud.com/server/15/admin_manual/installation/source_installation.html
###
ARG NC_VER=19.0.3
ARG NC_URL=https://download.nextcloud.com/server/releases/nextcloud-
ARG GPG_nextcloud="2880 6A87 8AE4 23A2 8372 792E D758 99B9 A724 937A"
RUN set -ex; \
    apk add --no-cache --virtual build-dependencies \
    # this is for downloading
    ca-certificates \
    openssl \
    curl \
    gnupg ;\
    update-ca-certificates ;\
    #get nextcloud and check signatures
    cd /tmp ; \
    curl -O ${NC_URL}${NC_VER}.tar.bz2.sha256 ; \
    curl -O ${NC_URL}${NC_VER}.tar.bz2.asc ; \
    curl -O ${NC_URL}${NC_VER}.tar.bz2.md5 ; \
    curl -O  ${NC_URL}${NC_VER}.tar.bz2 ; \
    md5sum -c nextcloud-${NC_VER}.tar.bz2.md5 < nextcloud-${NC_VER}.tar.bz2.md5 ;\
    sha256sum -c nextcloud-${NC_VER}.tar.bz2.sha256 < nextcloud-${NC_VER}.tar.bz2.sha256 ;\
    curl -O https://nextcloud.com/nextcloud.asc ;\
    gpg --import nextcloud.asc ;\
    FINGERPRINT="$(LANG=C gpg --verify nextcloud-${NC_VER}.tar.bz2.asc nextcloud-${NC_VER}.tar.bz2 2>&1 \
    | sed -n "s#Primary key fingerprint: \(.*\)#\1#p" | sed 's/  / /')";\
    if [ "${FINGERPRINT}" != "${GPG_nextcloud}" ]; then echo "Warning! Wrong GPG fingerprint!" && exit 1; fi ;\
    mkdir /nextcloud ;\
    apk del --purge build-dependencies ;\
    tar jxf /tmp/nextcloud-${NC_VER}.tar.bz2 -C /; \
    rm -rf /root/.gnupg ; \
    rm /tmp/nextcloud-${NC_VER}.tar.bz2
LABEL description="Nextcloud base" \
      nextcloud="Nextcloud-core v${NC_VER}" \
      maintainer="Appelo Solutions <tob@nice.eu>"
    
