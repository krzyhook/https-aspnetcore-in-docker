FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY HttpsAspNetCore/HttpsAspNetCore.csproj HttpsAspNetCore/
RUN dotnet restore HttpsAspNetCore/HttpsAspNetCore.csproj
COPY . .
WORKDIR /src/HttpsAspNetCore
RUN dotnet build HttpsAspNetCore.csproj -c Release -o /app

RUN dotnet publish HttpsAspNetCore.csproj -c Release -o /app

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
#EXPOSE 59709
EXPOSE 44309

COPY --from=build /app .


ARG CERT_PATH_DEST=/app/cert/localhost-dev.pfx
ENV CertPath=$CERT_PATH_DEST

RUN echo "Env CertPath value is: $CertPath"

COPY Cert/localhost-dev.pfx $CertPath

# ENV ASPNETCORE_URLS https://+:44320
# ENV ASPNETCORE_HTTPS_PORT 44320


ENTRYPOINT ["dotnet", "HttpsAspNetCore.dll"]
