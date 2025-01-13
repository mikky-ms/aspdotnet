# Base image untuk runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5196

# Konfigurasi environment
ENV ASPNETCORE_URLS=http://+:5196

# User yang digunakan
USER app

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILDPLATFORM=linux/amd64
ARG configuration=Release

# Direktori kerja untuk source code
WORKDIR /src

# Salin file proyek dan restore dependency
COPY ["TodoApi.csproj", "./"]
RUN dotnet restore "TodoApi.csproj"

# Salin semua file ke container dan lakukan build
COPY . .
RUN dotnet build "TodoApi.csproj" -c $configuration -o /app/build

# Publish stage
FROM build AS publish
ARG configuration=Release
RUN dotnet publish "TodoApi.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

# Final stage untuk runtime
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TodoApi.dll"]
