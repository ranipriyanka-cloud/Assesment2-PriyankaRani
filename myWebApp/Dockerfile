FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5104

ENV ASPNETCORE_URLS=http://+:5104

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Dotnet-Project/myWebApp/myWebApp.csproj", "Dotnet-Project/myWebApp/"]
RUN dotnet restore "Dotnet-Project/myWebApp/myWebApp.csproj"
COPY . .
WORKDIR "/src/Dotnet-Project/myWebApp"
RUN dotnet build "myWebApp.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "myWebApp.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "myWebApp.dll"]
