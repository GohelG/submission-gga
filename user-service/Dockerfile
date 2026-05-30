# 1 Copy Base Image
FROM node:16-slim

# 2 Create app directory
WORKDIR /app

# 3 Copy dependencies  
COPY package.json ./

# 4 Install dependencies 
RUN npm install

# 5 Copy app source code
COPY . .

# 6 Expose the port the app runs on
EXPOSE 3000

# 7 Start the app
CMD ["npm", "start"]