FROM python:3.7-slim@sha256:48876a8a7db7ce3af5836ac638dc202f4589d300dc666f1d2507ddcda6cb5ce4

RUN apt update && \
  apt-get install --no-install-recommends -y \
    supervisor \
    git && \
  pip install \
    --no-cache-dir \
    cryptography===37.0.4 \
    poetry===1.1.13 && \
  poetry config virtualenvs.in-project true && \
  groupadd kodiak && \
  useradd --uid 1000 --gid kodiak kodiak && \
  mkdir -p /var/app && \
  chown -R kodiak:kodiak /var/app

WORKDIR /var/app

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

COPY --chown=kodiak pyproject.toml poetry.lock /var/app

# install deps
RUN poetry install

COPY --chown=kodiak . /var/app

USER kodiak

CMD ["/usr/bin/supervisord"]