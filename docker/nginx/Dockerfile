# Let"s encrypt用のコンテナ
FROM nginx:1.27-alpine3.21 AS builder

# Let"s encrypt のパラメータ
ARG SSL_HOSTNAME
ARG EMAIL_ADDRESS
# Route53 の操作に必要なAWSアクセスキー
ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY

# コンテナに渡す最低限の環境変数をセット
ENV SSL_HOSTNAME=${SSL_HOSTNAME}
ENV AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
ENV AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}

# Let"s encrypt で証明書を発行
RUN apk upgrade && \
  apk add --no-cache certbot certbot-dns-route53 && \
  apk cache clean && \
    certbot certonly -d ${SSL_HOSTNAME} --email ${EMAIL_ADDRESS}\
    --agree-tos --dns-route53 --non-interactive --force-renewal

FROM nginx:1.27-alpine3.21
ARG CERTBOT_ROOT=/etc/letsencrypt
COPY --from=builder ${CERTBOT_ROOT}/live ${CERTBOT_ROOT}/live
COPY --from=builder ${CERTBOT_ROOT}/archive ${CERTBOT_ROOT}/archive