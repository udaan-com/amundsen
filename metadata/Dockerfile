FROM python:3.7-slim
WORKDIR /app
RUN pip3 install gunicorn

RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install git -y

COPY ./ /app
COPY requirements-dev.txt /app/requirements-dev.txt
COPY requirements-common.txt /app/requirements-common.txt

ENV FLASK_APP_MODULE_NAME flaskoidc
ENV FLASK_APP_CLASS_NAME FlaskOIDC
ENV FLASK_OIDC_WHITELISTED_ENDPOINTS status,healthcheck,health
ENV SQLALCHEMY_DATABASE_URI sqlite:///sessions.db

RUN  pip3 install -e .&& \
     pip3 install -e .[oidc]

CMD [ "python3", "metadata_service/metadata_wsgi.py" ]
