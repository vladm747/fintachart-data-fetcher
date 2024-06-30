FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FintachartsDataFetcher.API/FintachartsDataFetcher.API.csproj", "FintachartsDataFetcher.API/"]
RUN dotnet restore "FintachartsDataFetcher.API/FintachartsDataFetcher.API.csproj"
COPY . .
WORKDIR "/src/FintachartsDataFetcher.API"
RUN dotnet build "FintachartsDataFetcher.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "FintachartsDataFetcher.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FintachartsDataFetcher.API.dll"]
