# Don't use name alias. it will fain when dep[loy to AWS.
# FROM node:alpine as builder

# When running in production, we don't need to concern about the volume anymore 
# because we're not changing our code at all.

# Build Phase
FROM node:alpine
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
# Its output is the build folder in the working directory
# Our production assets that we want to serve to the outside world
# The path inside the container '/app/build' includes all the thing we want to copy over to the "Run Phase"
RUN npm run build

# Run Phase
# All we care is the 'app/build' directory which is the result from the Build Phase
# Normally, we could only have a single 'FROM', but the 'FROM' will be terminate after its succession.
FROM nginx
# Expose instruction tells devs to read inside the docker, and it does nothing.
# However, when Elastic Beanstalk starts up our docker container, it's going to look for 
# the Dockerfile and the EXPOSE instruction (to map the port automatically fro incoming traffic).
EXPOSE 80 
# COPY --from=builder /app/build
# according to nginx doc, copy to herer, '/usr/share/nginx/html'
COPY --from=0 /app/build /usr/share/nginx/html

# When we start up the nginx container, it's gonna run automatically without the need to start nginx manually