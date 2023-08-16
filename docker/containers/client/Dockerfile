# Technically we do not need nginx here, we could simply use node but oh well.
# In this case, nginx is used to keep the container running.
FROM nginx:1.23.2-alpine

ENV NODE_OPTIONS=--max-old-space-size=8048

RUN apk add --update --no-cache --upgrade bash grep vim nano mc yarn git
RUN yarn global add pm2

WORKDIR /var/www

COPY ./default.conf /etc/nginx/conf.d

CMD yarn && yarn build && pm2 start ecosystem.config.js && nginx -g 'daemon off;'
