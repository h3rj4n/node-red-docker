FROM arm64v8/node:8-alpine

# Experimental ARMv8 support for Node-RED

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN apk --update --no-cache add python docker git \
    && apk --update --no-cache add --virtual .build-dependencies python-dev make g++ autoconf \
    && npm install bcrypt@2.0.1 \
    && apk del .build-dependencies \
    && adduser -h /usr/src/node-red -H -D node-red \
    && addgroup node-red ping \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red

USER node-red

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
RUN npm install

# User configuration directory volume
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json

CMD ["npm", "start", "--", "--userDir", "/data"]
