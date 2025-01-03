# Use an official lightweight Debian base image for ARM architecture
FROM arm32v7/debian:bullseye-slim

# Set environment variables to non-interactive
ENV DEBIAN_FRONTEND=noninteractive

# Update and install necessary packages
RUN apt-get update && apt-get install -y \
    xfce4 \
    xfce4-goodies \
    tightvncserver \
    libreoffice-impress \
    supervisor \
    dbus-x11 \
    x11-xserver-utils \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create a user to run the application
RUN useradd -m dockeruser && echo "dockeruser:docker" | chpasswd && adduser dockeruser sudo

# Set the user
USER dockeruser
ENV USER=dockeruser
WORKDIR /home/dockeruser

# Setup VNC server
RUN mkdir ~/.vnc
RUN echo "#!/bin/sh\n\
xrdb $HOME/.Xresources\n\
startxfce4 &" > ~/.vnc/xstartup && chmod +x ~/.vnc/xstartup

# Expose VNC port
EXPOSE 5901

# Copy Supervisor configuration
COPY docker/vnc/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Start Supervisor to manage VNC
CMD ["/usr/bin/supervisord"]
