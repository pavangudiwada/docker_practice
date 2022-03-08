FROM node:lts-alpine3.14
WORKDIR /app

COPY package.json ./
COPY package-lock.json ./
RUN npm install 
RUN npm install react-scripts@5.0.0 -g 

COPY . ./

CMD ["npm","start"]
