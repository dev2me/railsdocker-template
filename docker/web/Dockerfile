# Base image
FROM nginx
# Install dependencies
RUN apt-get update -qq && apt-get -y install apache2-utils
# establish where Nginx should look for files
ENV RAILS_ROOT /app

# Set our working directory inside the image
WORKDIR $RAILS_ROOT

# Create log directory
RUN mkdir /app/log

# Copy over static assets
COPY public /app/public/

# Copy nginx config template
COPY docker/web/nginx.conf /tmp/docker.nginx

# Substitute variable references in the Nginx config template for real values from the environment

RUN envsubst '$RAILS_ROOT' < /tmp/docker.nginx > /etc/nginx/conf.d/default.conf

# Expose port
EXPOSE 80

# Run process nginx
CMD ["nginx", "-g", "daemon off;"]
