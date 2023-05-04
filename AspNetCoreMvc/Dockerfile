#1. image
FROM mcr.microsoft.com/dotnet/sdk:7.0 as buildName
WORKDIR /app

#Sadece csproj dosyasýný app içine kopyala eklememizin amacý bir controller eklediðimizde tamamen baþtan iþlem yapmasýný istemediðimizden kaynaklanmaktadýr. Cacheden gelecek. wwwrootta deðiþiklik olduðunda projenin diðer dosyalarý tekrar oluþturulmayacak cacheden gelecektir.
COPY *.csproj .
RUN dotnet restore
#bununla birlikte statik bir dosya kopyalanacak.
COPY . . 
RUN  dotnet restore
RUN  dotnet publish AspNetCoreMvc.csproj -c Release -o out

#2.image
##aþaðýdaki ve yukarýdaki imajlar ayrýlabilir.
##yukarýda out içine publish edilmiþ dosyalarý aþaðýdaki imagede taþýmam gerekiyor
#faydasý sdk deki büyük imagedan kurtulmuþ oluyorum.
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
COPY --from=buildName /app/out .
ENV ASPNETCORE_URLS="http://*:4500"
ENV ASPNETCORE_ENVIRONMENT="DEVELOPMENT"
ENV MySqlCon="Uzak Sunucu Veritabaný Yolu"
ENTRYPOINT ["dotnet","AspNetCoreMvc.dll"]