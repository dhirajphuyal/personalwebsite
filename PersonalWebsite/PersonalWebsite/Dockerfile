FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["PersonalWebsite/PersonalWebsite/PersonalWebsite.csproj", "PersonalWebsite/PersonalWebsite/"]
COPY ["PersonalWebsite/PersonalWebsite.Client/PersonalWebsite.Client.csproj", "PersonalWebsite/PersonalWebsite.Client/"]
RUN dotnet restore "PersonalWebsite/PersonalWebsite/PersonalWebsite.csproj"
COPY . .
WORKDIR "/src/PersonalWebsite/PersonalWebsite"
RUN dotnet build "./PersonalWebsite.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./PersonalWebsite.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PersonalWebsite.dll"]
