#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
#WORKDIR /app
#EXPOSE 80
#EXPOSE 443
#
#FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
#WORKDIR /src
#COPY ["aws-lambda-apigateway-dotnet7/aws-lambda-apigateway-dotnet7.csproj", "aws-lambda-apigateway-dotnet7/"]
#RUN dotnet restore "aws-lambda-apigateway-dotnet7/aws-lambda-apigateway-dotnet7.csproj"
#COPY . .
#WORKDIR "/src/aws-lambda-apigateway-dotnet7"
#RUN dotnet build "aws-lambda-apigateway-dotnet7.csproj" -c Release -o /app/build
#
#FROM build AS publish
#RUN dotnet publish "aws-lambda-apigateway-dotnet7.csproj" -c Release -o /app/publish /p:UseAppHost=false
#
#FROM base AS final
#WORKDIR /app
#COPY --from=publish /app/publish .
#ENTRYPOINT ["dotnet", "aws-lambda-apigateway-dotnet7.dll"]

FROM public.ecr.aws/lambda/dotnet:7 AS base

FROM mcr.microsoft.com/dotnet/sdk:7.0-bullseye-slim as build
WORKDIR /src
COPY ["aws-lambda-apigateway-dotnet7/aws-lambda-apigateway-dotnet7.csproj", "aws-lambda-apigateway-dotnet7/"]
RUN dotnet restore "aws-lambda-apigateway-dotnet7/aws-lambda-apigateway-dotnet7.csproj"
COPY . .
WORKDIR "/src/aws-lambda-apigateway-dotnet7"
RUN dotnet build "aws-lambda-apigateway-dotnet7.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "aws-lambda-apigateway-dotnet7.csproj" \
            -c Release \ 
            -r linux-x64 \
            --sc false \ 
            -o /app/publish \
            -p:PublishReadyToRun=true  

FROM base AS final
WORKDIR /var/task
COPY --from=publish /app/publish .
RUN ls -alR