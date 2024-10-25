FROM node:17-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# We are exposing port 3000 since react apps run on it by default
EXPOSE 3000
# required for docker desktop port mapping

CMD ["npm", "start"]