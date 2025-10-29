FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["dataarchaive/dataarchaive.csproj", "dataarchaive/"]
RUN dotnet restore "dataarchaive/dataarchaive.csproj"
COPY . .
WORKDIR "/src/dataarchaive"
RUN dotnet build "dataarchaive.csproj" -c Release -o /app/build

# Stage 2: Publish the application
FROM build AS publish
RUN dotnet publish "dataarchaive.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Stage 3: Use the .NET 8 ASP.NET runtime for the final image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
EXPOSE 80
EXPOSE 443
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dataarchaive.dll"]
