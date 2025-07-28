# ---- Build Stage ----
FROM eclipse-temurin:21-jdk AS build

# Set working directory inside the container
WORKDIR /usr/src/app/

# Copy Gradle wrapper, settings, and build files first (for better caching)
COPY gradlew* settings.gradle* build.gradle ./
COPY ./gradle ./gradle

# Make Gradle wrapper executable
RUN chmod +x ./gradlew

# Download Gradle dependencies (to leverage Docker layer caching)
RUN ./gradlew
RUN ./gradlew downloadRepos

# Copy the rest of the source code # use COPY src/ src/ instead
COPY . .

# Copy proto files to correct path
COPY ./pb ./proto

# Ensure wrapper is executable after copy
RUN chmod +x ./gradlew

# Build application distribution
RUN ./gradlew installDist -PprotoSourceDir=./proto

# ---- Release Stage ----
FROM gcr.io/distroless/java21-debian12:nonroot

# Set working directory
WORKDIR /usr/src/app/

# Copy build artifacts from the build stage
COPY --from=build /usr/src/app/ .

# Change ownership of files so the non-root user can access them
RUN chown -R java-app:java /usr/src/app

# Switch to non-root user for security
USER nonroot

# Environment variables
ENV AD_PORT=8080
ENV FEATURE_FLAG_GRPC_SERVICE_ADDR=featureflagservice:50053

# Expose the port (optional, for documentation and tooling)
EXPOSE 8080

# Entry point - start the Java application
ENTRYPOINT ["./build/install/opentelemetry-demo-ad/bin/Ad"]
