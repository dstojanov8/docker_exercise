FROM node:17-alpine

# nodemon restarts the node server once it detects a change
RUN npm install -g nodemon

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .
  
EXPOSE 4000

# we created this script in package.json file to run nodemon
CMD ["npm", "run", "dev"]