FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081


FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["RaccoonEcommerceAPI/RaccoonEcommerceAPI.csproj", "RaccoonEcommerceAPI/"]
RUN dotnet restore "./RaccoonEcommerceAPI/RaccoonEcommerceAPI.csproj"
COPY . .
WORKDIR "/src/RaccoonEcommerceAPI"
RUN dotnet build "./RaccoonEcommerceAPI.csproj" -c %BUILD_CONFIGURATION% -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./RaccoonEcommerceAPI.csproj" -c %BUILD_CONFIGURATION% -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RaccoonEcommerceAPI.dll"]