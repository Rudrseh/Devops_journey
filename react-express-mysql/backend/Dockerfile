FROM node:20 AS development

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /code

ARG PORT=80
ENV PORT=${PORT}
EXPOSE ${PORT} 9229 9230    

COPY package*.json ./

RUN npm ci

HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 CMD curl -f http://localhost:${PORT}/health || exit 1 \
CMD node healthcheck.js

COPY . ./code

CMD ["npm", "src/index.js"]

FROM development AS dev-envs

RUN <<EOF
  apt-get update && 
  apt-get install -y --no-install-recommends 
  git
EOF

RUN <<EOF
  useradd -s /bin/bash -m vscode
  grouadd docker
  usermod -aG docker vscode
EOF

COPY --from=gloursdocker/docker / /
