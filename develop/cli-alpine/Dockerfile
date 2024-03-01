FROM alpine:3.19 AS base
# Build OpenRCT2
FROM base AS build-env
RUN apk add --no-cache git gcc g++ make cmake nlohmann-json libzip-dev curl-dev fontconfig-dev icu-dev musl-dev linux-headers

ARG OPENRCT2_REF=develop
WORKDIR /openrct2
RUN git -c http.sslVerify=false clone --depth 1 -b $OPENRCT2_REF https://github.com/OpenRCT2/OpenRCT2 . \
 && mkdir build \
 && cd build \
 && cmake .. -DCMAKE_CXX_COMPILER=/usr/bin/g++ -DCMAKE_BUILD_TYPE=release -DCMAKE_INSTALL_PREFIX=/openrct2-install/usr -DCMAKE_INSTALL_LIBDIR=/openrct2-install/usr/lib -DDISABLE_OPENGL=ON -DDISABLE_GUI=ON \
 && make -j4 install \
 && rm /openrct2-install/usr/lib/libopenrct2.a \
 # HACK due to issue in cmakelists, move content from cli
 && mv /openrct2-install/usr/share/openrct2-cli/* /openrct2-install/usr/share/openrct2 \
 && rm -rf /openrct2-install/usr/share/openrct2-cli

# Build runtime image
FROM base
HEALTHCHECK  --timeout=3s --interval=120s \
  CMD [[ -z "$(ps aux | grep openrct2-cli | grep host | grep -v grep)" ]] || \
      [[ -n "$(ps aux | grep openrct2-cli | grep -- "--port" | grep -v grep)" ]] || \
      [[ -n "$(netstat -tlnp | grep openrct2-cli | grep "$(cat '.config/OpenRCT2/config.ini' | grep 'default_port' | awk '{print $NF}' )" )" ]]  || \
      exit 1
COPY --from=build-env /openrct2-install /openrct2-install
RUN apk add --no-cache rsync ca-certificates libpng libzip libcurl freetype fontconfig icu \
 && rsync -a /openrct2-install/* / \
 && rm -rf /openrct2-install \
 && openrct2-cli --version \
# Set up ordinary user
 && addgroup -g 1000 -S openrct2 && adduser -G openrct2 -u 1000 -DS openrct2
USER openrct2
WORKDIR /home/openrct2
EXPOSE 11753

# Test run and scan
RUN openrct2-cli --version \
 && openrct2-cli scan-objects

# Done
ENTRYPOINT ["openrct2-cli"]