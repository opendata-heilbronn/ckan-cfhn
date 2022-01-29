FROM python:3.8

ENV LC_ALL=C
WORKDIR /usr/lib/ckan/datapusher
ENV VENV /usr/lib/ckan/datapusher
RUN python3 -m venv $VENV

WORKDIR /usr/lib/ckan/datapusher/src/datapusher
COPY requirements.txt ./
RUN $VENV/bin/pip install uwsgi && \
    $VENV/bin/pip install -r requirements.txt
COPY . .

RUN sed -i s/127.0.0.1:8800/0.0.0.0:8800/g /usr/lib/ckan/datapusher/src/datapusher/deployment/datapusher-uwsgi.ini
RUN echo <<EOF >>/usr/lib/ckan/datapusher/src/datapusher/deployment/datapusher-uwsgi.ini \
workers         =  3 \
threads         =  3 \
lazy-apps       =  true \
EOF

RUN $VENV/bin/python setup.py develop

CMD ["/usr/lib/ckan/datapusher/bin/uwsgi", "-i", "/usr/lib/ckan/datapusher/src/datapusher/deployment/datapusher-uwsgi.ini"]
