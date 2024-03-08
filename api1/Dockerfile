FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
ENV ASPNETCORE_HTTP_PORTS=80
COPY . ./
RUN dotnet restore api1.csproj
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
ENV ASPNETCORE_HTTP_PORTS=80
COPY --from=build /app/out .
ENTRYPOINT ["dotnet", "api1.dll"]