#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs
WORKDIR /src
COPY ["src/AccountGoWeb/AccountGoWeb.csproj", "src/AccountGoWeb/"]
COPY ["src/Infrastructure/Infrastructure.csproj", "src/Infrastructure/"]
COPY ["src/Dto/Dto.csproj", "src/Dto/"]
RUN dotnet restore "src/AccountGoWeb/AccountGoWeb.csproj"
COPY . .
WORKDIR "/src/src/AccountGoWeb"
RUN dotnet build "AccountGoWeb.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AccountGoWeb.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AccountGoWeb.dll"]
