#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["AnyGym.API/AnyGym.API.csproj", "AnyGym.API/"]
COPY ["AnyGym.CrossCutting/AnyGym.CrossCutting.csproj", "AnyGym.CrossCutting/"]
COPY ["AnyGym.Application/AnyGym.Application.csproj", "AnyGym.Application/"]
COPY ["AnyGym.Domain/AnyGym.Domain.csproj", "AnyGym.Domain/"]
COPY ["AnyGym.Infrastructure/AnyGym.Infrastructure.csproj", "AnyGym.Infrastructure/"]
RUN dotnet restore "./AnyGym.API/AnyGym.API.csproj"
COPY . .
WORKDIR "/src/AnyGym.API"
RUN dotnet build "./AnyGym.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./AnyGym.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AnyGym.API.dll"]