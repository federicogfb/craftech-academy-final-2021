FROM node:12
ENV NODE_ENV=development
ENV BABEL_ENV=development
ENV HOST=0.0.0.0
ENV PORT=3000

WORKDIR /usr/src/app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm -v \
    && npm install -g npm@6\
    && npm -v
RUN npm install

COPY . /usr/src/app

EXPOSE 3000
CMD ["npm", "run", "start"]