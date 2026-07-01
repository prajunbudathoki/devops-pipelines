# set the base image to create the image for react app
FROM node:20-alpine

# create a user with permission to run the app
# -s -> create a system user 
# -G -> add the user to a group
# This is done to avoid running the app as root user
# If the app is run as root, it can have access to the entire system and can cause security issues
# It's a good practice to run the app as a non-root user
RUN addgroup app && adduser -S -G app app

# set the user to run the app
USER app

# set the working directory to /app
WORKDIR /app

# copy the package.json and package-lock.json to the working directory
# This is done before copying the entire app to leverage Docker's caching mechanism. 
# If the package.json or package-lock.json files haven't changed, Docker will use the cached layer and won't reinstall the dependencies.
COPY package*.json ./

# sometimes the ownership of the files in the working directory is changed to root 
# and thus the app can't access the files and throws an error -> EACCES: permission denied
# to avoid this, change the ownership of the files to the root user 
USER root

# change the ownership of the files to the root user
USER ROOT

# change the ownership of the app/directory to the app user
# chown -R <user>:<group> <directory>
# chown commands changes the user and/or group ownership of for given files.
RUN chown -R app:app 

# change the user back to app user
USER app

# install dependencies
RUN npm install

# copy the rest of the files to the working directory
COPY . .

# expose port 5173 to tell Docker that the container listens on the specified network ports at runtime
EXPOSE 5173

# command to run the app
CMD npm run dev
