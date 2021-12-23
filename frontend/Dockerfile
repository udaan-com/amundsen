FROM node:12-slim as node-stage
WORKDIR /app/amundsen_application/static

COPY ./amundsen_application/static/package.json /app/amundsen_application/static/package.json
COPY ./amundsen_application/static/package-lock.json /app/amundsen_application/static/package-lock.json
RUN npm install

COPY ./amundsen_application/static /app/amundsen_application/static
RUN npm run build

FROM python:3.7-slim
WORKDIR /app
RUN pip3 install gunicorn

ENV FRONTEND_SVC_CONFIG_MODULE_CLASS amundsen_application.oidc_config.OidcConfig
ENV APP_WRAPPER flaskoidc
ENV APP_WRAPPER_CLASS FlaskOIDC
ENV FLASK_OIDC_WHITELISTED_ENDPOINTS status,healthcheck,health
ENV SQLALCHEMY_DATABASE_URI sqlite:///sessions.db


COPY --from=node-stage /app /app
COPY ./ /app
COPY ../requirements-dev.txt /app/requirements-dev.txt
COPY ../requirements-common.txt /app/requirements-common.txt

RUN pip3 install -e .[oidc] && \
    pip3 install -e .

CMD [ "python3",  "amundsen_application/wsgi.py" ]
