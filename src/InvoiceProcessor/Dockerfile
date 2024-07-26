FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["InvoiceProcessor/InvoiceProcessor.csproj", "InvoiceProcessor/"]
RUN dotnet restore "InvoiceProcessor/InvoiceProcessor.csproj"
COPY . .
WORKDIR "/src/InvoiceProcessor"
RUN dotnet build "InvoiceProcessor.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "InvoiceProcessor.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "InvoiceProcessor.dll"]
