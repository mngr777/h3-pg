ARG UBUNTU=focal
FROM ubuntu:$UBUNTU

ARG UBUNTU
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    build-essential \
    # cmake \ # see following comments
    git \
    pgxnclient

# H3 requires CMake 3.20, which has not landed in ubuntu
# as of writing. So we setup cmake apt repository
RUN if [ "$UBUNTU" != "impish" ] ; then apt-key adv --fetch-keys https://apt.kitware.com/keys/kitware-archive-latest.asc ; fi
RUN if [ "$UBUNTU" != "impish" ] ; then echo "deb https://apt.kitware.com/ubuntu/ ${UBUNTU} main" >> /etc/apt/sources.list.d/pgdg.list ; fi
RUN apt-get update && apt-get install -y cmake
# we can remove the block above when 3.20 lands

# Setup PostgreSQL apt repository
RUN apt-key adv --fetch-keys https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ ${UBUNTU}-pgdg main" >> /etc/apt/sources.list.d/pgdg.list

ARG POSTGRESQL=14
ARG POSTGIS=3

RUN apt-get update && apt-get install -y \
    postgresql-${POSTGRESQL}-postgis-${POSTGIS}-scripts \
    postgresql-${POSTGRESQL}-postgis-${POSTGIS} \
    postgresql-server-dev-${POSTGRESQL} \
    postgresql-${POSTGRESQL}

# Set workdir
WORKDIR /github/workspace

COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
