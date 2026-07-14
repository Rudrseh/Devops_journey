FROM node:20 AS development

ENV CI=true
ENV PORT=3000

WORKDIR /code

COPY package.json package-lock.json ./

RUN npm ci

COPY . ./code

CMD ["npm", "start"]

FROM development AS build

RUN npm run build

FROM development AS dev-envs

RUN <<EOF
apt-get update && apt-get install -y  --no-install-recommends 
    git
EOF

RUN <<EOF
   useradd -ms /bin/bash vscode
   groupadd docker
   usermod -aG docker vscode
EOF

COPY --from=gloursdocker/docker / /

CMD ["npm", "start"]

FROM nginx:alpine

COPY --from=build /code/build /usr/share/nginx/html