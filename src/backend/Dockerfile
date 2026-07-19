# configure docker file nodejs :- backend 
FROM nodejs:18-alpine 

WORKDIR /src

COPY package*.json ./

RUN Npm install

COPY . .

EXPOSE 5000
CMD ["node", "server.js"] 