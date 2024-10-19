FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FinanceTrackerNet.Security/FinanceTrackerNet.Security.csproj", "FinanceTrackerNet.Security/"]
RUN dotnet restore "FinanceTrackerNet.Security/FinanceTrackerNet.Security.csproj"
COPY . .
WORKDIR "/src/FinanceTrackerNet.Security"
RUN dotnet build "FinanceTrackerNet.Security.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "FinanceTrackerNet.Security.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FinanceTrackerNet.Security.dll"]
