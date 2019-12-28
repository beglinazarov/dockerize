#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["WebAppOnDocker/WebAppOnDocker.csproj", "WebAppOnDocker/"]
RUN dotnet restore "WebAppOnDocker/WebAppOnDocker.csproj"
COPY . .
WORKDIR "/src/WebAppOnDocker"
RUN dotnet build "WebAppOnDocker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebAppOnDocker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAppOnDocker.dll"]