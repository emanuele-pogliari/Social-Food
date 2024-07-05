FROM node:lts

EXPOSE 3000
# Set the working directory
WORKDIR /home/node/app
# Copy package files and install dependencies
COPY package.json ./
COPY package-lock.json ./
RUN npm install
RUN npm install -g npm@10.8.1

# Copy the rest of the application code
COPY . .

# Adjust permissions
RUN chown -R node:node /home/node/app
RUN chmod -R u+w /home/node/app

# Switch to non-root user
USER node
CMD ["npm", "run", "dev"]