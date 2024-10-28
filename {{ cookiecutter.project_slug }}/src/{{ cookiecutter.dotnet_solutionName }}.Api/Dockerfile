FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["{{ cookiecutter.dotnet_solutionName }}.Api/{{ cookiecutter.dotnet_solutionName }}.Api.csproj", "{{ cookiecutter.dotnet_solutionName }}.Api/"]
RUN dotnet restore "{{ cookiecutter.dotnet_solutionName }}.Api/{{ cookiecutter.dotnet_solutionName }}.Api.csproj"
COPY . .
WORKDIR "/src/{{ cookiecutter.dotnet_solutionName }}.Api"
RUN dotnet build "{{ cookiecutter.dotnet_solutionName }}.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "{{ cookiecutter.dotnet_solutionName }}.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "{{ cookiecutter.dotnet_solutionName }}.Api.dll"]
