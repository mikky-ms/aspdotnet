# Base image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5196

# Configure environment variable for URL binding
ENV ASPNETCORE_URLS=http://+:5196

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILDPLATFORM=linux/amd64
ARG configuration=Release

# Set working directory for source code
WORKDIR /src

# Copy project file and restore dependencies
COPY ["TodoApi.csproj", "./"]
RUN dotnet restore "TodoApi.csproj"

# Copy all files and build the application
COPY . .
RUN dotnet build "TodoApi.csproj" -c $configuration -o /app/build

# Publish stage
FROM build AS publish
ARG configuration=Release
RUN dotnet publish "TodoApi.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

# Final stage for runtime
FROM base AS final
WORKDIR /app

# Copy published application from publish stage
COPY --from=publish /app/publish .

# Set default environment variable
ENV ASPNETCORE_ENVIRONMENT=Production

# Application entry point
ENTRYPOINT ["dotnet", "TodoApi.dll"]
