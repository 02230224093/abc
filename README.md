1. Hocanın istediği github linkine git . Zip olarak indir ve onu bir dosyaya çıkart. Burda klasör içine gir ordaki dosyaları kopyala klasörü değil. Git olan klasöre dokunma içindekini de alma
2. Githubta bir repo aç ve clone repository yerinden boş reponu bağla
3. Klasör içine lib diye bir dosya açıp içine postgresql jar dosyasını ekle.
4. Kodların hepsi projende var dosya olarak build/web olan yere gel Dockerfile aç kodları yaz.
5. Dockerfile'ı çalıştırmak için openfolder yapıp buildin altındaki webe gel. Eğer terminal derse cd build/web
touch Dockerfile şunları sırasıyla yaz nano ekranı açılır kodu oraya yapıştır.
Alttaki postgre kodlarını yaptıktan sonra 6. adıma geç.
6. terminale docker build -t ymsfinal .
7. docker run -p 8090:8080 -p 4849:4848 ymsfinal
8. localhost:8090/HelloWeb/
9. Çakışma olmasın diye imaj ve konteynerleri sil . Elle silmee izin vermese terminalden ;
docker rmi -f $(docker images -q)
 docker stop $(docker ps -aq)
 docker rm $(docker ps -aq)
 sırasıyla çalıştır.
Daha sonra terminalde docker-compose up -d yap
localhost/HelloWeb/ sayfasını aç
Kodları githuba atmak için commit commit al publish branch yapman yeterli. 
 
 


Veritabanı bağlantısı için ; Proje içine git notepad dosyası aç yapıştır.
docker-compose.yml dosyası 

version: '3'

services:
  postgres:
    image: postgres:15
    container_name: postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: userdb
    ports:
      - "5432:5432"
    networks:
      - mynetwork
    volumes:
      - pgdata:/var/lib/postgresql/data

  glassfish:
    build: .
    container_name: cmdymgfinal-container
    ports:
      - "8090:8080"
      - "4849:4848"
    networks:
      - mynetwork
    depends_on:
      - postgres

  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - glassfish
    networks:
      - mynetwork

networks:
  mynetwork:

volumes:
  pgdata:


  terminale docker exec -it postgres psql -U postgres -d userdb bunu yaz.
  daha sonra sql komutları için ekran açılacak 
  CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO users (username, email) VALUES('esra','esra@example.com');
SELECT * FROM users;
bu sorguları yazdıktan sonra /q ile çıkış yap



githuba atmak için 
cd klasör
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/02230224093/abcd.git
git push -u origin main
