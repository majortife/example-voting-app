FROM node:22-bookworm-slim@sha256:sha256:de170b39439eb0e5a3444ad7d60b015614a381e10fecae2e30ec9e735122f38d

# Install curl for health checks and tini for signal handling
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl tini && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/app

COPY package*.json ./

# Install production dependencies only
RUN npm ci --omit=dev && \
    npm cache clean --force && \
    mv /usr/local/app/node_modules /node_modules

COPY . .

# Give the existing node user ownership of application files
RUN chown -R node:node /usr/local/app /node_modules

ENV PORT=80
EXPOSE 80

USER node

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["node", "server.js"]