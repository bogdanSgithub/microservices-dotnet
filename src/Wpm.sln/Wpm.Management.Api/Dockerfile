# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Base runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy solution file
COPY ./Wpm.sln/Wpm.sln.sln ./Wpm.sln.sln

# Copy project files
COPY ./Wpm.sln/Wpm.Management.Api/Wpm.Management.Api.csproj ./Wpm.Management.Api/

# Restore
RUN dotnet restore "./Wpm.Management.Api/Wpm.Management.Api.csproj"

# Copy all source
COPY ./Wpm.sln ./Wpm.sln

WORKDIR "/src/Wpm.sln/Wpm.Management.Api"

# Build
RUN dotnet build "Wpm.Management.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish stage
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Wpm.Management.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Wpm.Management.Api.dll"]
