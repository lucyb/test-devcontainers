FROM remlapmot/r-docker:2024-04-02-rstudio

# we are going to use an apt cache on the host, so disable the default debian
# docker clean up that deletes that cache on every apt install
RUN rm -f /etc/apt/apt.conf.d/docker-clean

# install renv
RUN --mount=type=cache,target=/cache,id=/cache-2004 R -e 'install.packages("renv", destdir="/cache"); renv::init(bare = TRUE)'

# copy the renv directory from the OpenSAFELY R action image
COPY --from=ghcr.io/opensafely-core/r /renv/ /renv/

# ensure fully working base python3.10 installation using deadsnakes ppa
RUN --mount=type=cache,target=/var/cache/apt \
    echo "deb http://ppa.launchpad.net/deadsnakes/ppa/ubuntu focal main" > /etc/apt/sources.list.d/deadsnakes-ppa.list &&\
    /usr/lib/apt/apt-helper download-file 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xf23c5a6cf475977595c89f51ba6932366a755776' /etc/apt/trusted.gpg.d/deadsnakes.asc

# Install python 3.10
RUN --mount=type=cache,target=/var/cache/apt \
    sudo apt update && sudo apt install -y curl python3.10 python3.10-distutils \
    # Pip for Python 3.10 isn't included in deadsnakes, so install separately
    && curl https://bootstrap.pypa.io/get-pip.py | python3.10

# Create a local user and give it sudo (aka root) permissions
#RUN useradd -ms /bin/bash opensafely && usermod -aG sudo opensafely
RUN usermod -aG sudo rstudio
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Required for installing opensafely cli
ENV PATH="/home/rstudio/.local/bin:${PATH}"

USER rstudio