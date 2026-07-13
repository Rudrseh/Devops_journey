FROM nginx:latest as development

WORKDIR /app

COPY package*.json ./app/

RUN npm ci

COPY . ./app

ENV CI=true
ENV port=3000

CMD ["npm", "start"]

FROM development as build

RUN npm run build

FROM development as dev-envs

RUN <<EOF
apt-get update && apt-get install -y --no-install-recommends git 
EOF

RUN <<EOF 
useradd -ms /bin/bash -m vscode
groupadd docker
usermod -aG docker vscode
EOF

COPY --from=gloursdocker/docker / /
CMD ["npm", "start"]

FROM nginx:latest as production

COPY --from=build /app/.nginx/nginx.conf /etc/nginx/conf.d/default.conf

WORKDIR /usr/share/nginx/html

RUN rm -rf ./*

COPY --from=build /app/build .

ENTRYPOINT ["nginx", "-g", "daemon off;"]


