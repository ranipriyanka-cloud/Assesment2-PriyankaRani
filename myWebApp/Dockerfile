FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5104

ENV ASPNETCORE_URLS=http://+:5104

RUN chown -R app:app /app
USER app

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["./myWebApp.csproj", "myWebApp/"]
RUN dotnet restore "myWebApp/myWebApp.csproj"
COPY . .
WORKDIR /src/
RUN dotnet build myWebApp.csproj -c $configuration --output /src/build /p:UseAppHost=false

FROM build AS publish
ARG configuration=Release
RUN dotnet publish myWebApp.csproj -c $configuration --output /src/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /src/publish .
ENTRYPOINT ["dotnet", "myWebApp.dll"]