FROM mcr.microsoft.com/dotnet/aspnet:8.0 as base
USER app
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 as build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY webapp.csproj .
RUN dotnet restore webapp.csproj
COPY . .
RUN dotnet build webapp.csproj -c ${BUILD_CONFIGURATION} -o /dest/build

FROM build as publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish webapp.csproj -c ${BUILD_CONFIGURATION} -o /dest/publish

FROM base as final
WORKDIR /app
COPY --from=publish /dest/publish .
ENTRYPOINT [ "dotnet", "webapp.dll" ]

