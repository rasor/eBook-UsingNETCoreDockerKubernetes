# Generated Dockerfile
# 1. Target img to deploy to
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 5000
EXPOSE 5001

# 5. Source img to build on
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
# 7. Copy project file from local pc into Source img
COPY ./frontend.csproj .
RUN dotnet restore frontend.csproj
COPY . .
WORKDIR /src
# 11. Build .dll
RUN dotnet build "frontend.csproj" -c Release -o /app/build

FROM build AS publish
# 13. Create a deployable package
RUN dotnet publish "frontend.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
# 16. Copy from Source img to Target img
COPY --from=publish /app/publish .
# 17. Pass in UseUrls
#ENV MVCAPPURLS=https://0.0.0.0:5001;http://0.0.0.0:5000
ENV MVCAPPURLS=http://0.0.0.0:5000
# 18. Define start command
ENTRYPOINT ["dotnet", "frontend.dll"]
