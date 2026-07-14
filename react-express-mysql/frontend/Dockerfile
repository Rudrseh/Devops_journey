<<<<<<< HEAD
# syntax=docker/dockerfile:1.4

FROM node:lts AS development

ENV CI=true
ENV PORT=3000

WORKDIR /code
COPY package.json /code/package.json
COPY package-lock.json /code/package-lock.json
RUN npm ci
COPY . /code

CMD [ "npm", "start" ]

FROM development AS builder

RUN npm run build

FROM development as dev-envs
RUN <<EOF
apt-get update
apt-get install -y --no-install-recommends git
EOF

RUN <<EOF
useradd -s /bin/bash -m vscode
groupadd docker
usermod -aG docker vscode
EOF
# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /
CMD [ "npm", "start" ]

FROM nginx:1.13-alpine

COPY --from=builder /code/build /usr/share/nginx/html
=======
FROM node:20 AS development

ENV CI=true
ENV PORT=3030 

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
>>>>>>> 8f216cbc6a4208b70433402eddba4015101d36fc
