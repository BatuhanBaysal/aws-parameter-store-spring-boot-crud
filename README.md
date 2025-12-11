# 🚀 Spring Boot Employee Directory (AWS Parameter Store Entegrasyonlu)

### Sunucu Taraflı İşleme (Server-Side Rendering) kullanılan bu proje, AWS Parameter Store ile hassas veritabanı bağlantı bilgilerini güvenli bir şekilde yöneten bir Çalışan Yönetim Sistemi (CRUD) uygulamasıdır.

## 🎯 Proje Amacı

Bu uygulama, temel çalışan kayıtlarının (Ad, Soyad, E-posta) web arayüzü üzerinden yönetilmesini (Oluşturma, Okuma, Güncelleme, Silme - CRUD) sağlar. Projenin teknik odak noktası, çalışan verilerini yönetmek ve hassas veritabanı kimlik bilgilerini (**MySQL**) güvenli bir şekilde **AWS Parameter Store** üzerinden çekerek **Cloud altyapısıyla entegrasyon** yetkinliğini göstermektir. Proje, katmanlı mimari ve temiz kodlama prensipleriyle tasarlanmıştır.

## ⚙️ Kullanılan Temel Teknolojiler

| Kategori | Teknoloji | Rolü | 
 | ----- | ----- | ----- | 
| **Backend Çekirdek** | **Spring Boot 3.4.0**, **Java 17** | Uygulamanın temel iş mantığı ve API servisleri. | 
| **Frontend** | **Thymeleaf**, **Bootstrap 5.2.2** | Sunucu tarafında dinamik HTML sayfaları oluşturma ve modern, duyarlı arayüz tasarımı. | 
| **Veritabanı** | **MySQL**, **Spring Data JPA** | Çalışan verilerinin depolanması ve ORM (Object-Relational Mapping) ile veri erişimi. | 
| **Cloud Entegrasyonu** | **AWS Parameter Store** | Hassas veritabanı bağlantı bilgilerini güvenli bir şekilde yükleme. | 
| **Araçlar** | **Maven**, **Spring DevTools** | Proje bağımlılık yönetimi ve hızlı yeniden yükleme. | 

## 🏗️ Proje Mimarisi

Uygulama, temiz ve bakımı kolay bir yapı sağlayan geleneksel **Katmanlı Mimari** kullanır:

1. **Controller:** HTTP isteklerini yönetir ve Thymeleaf şablonlarını kullanarak sunucu tarafından render edilen (SSR) sayfaları döndürür.

2. **Service:** İş mantığının uygulandığı katmandır. Veri işlemleri için Repository katmanını kullanır.

3. **DAO / Repository:** Veritabanı ile doğrudan etkileşim kuran katmandır. Spring Data JPA (`JpaRepository`) kullanılır.

4. **Entity:** Veritabanındaki `employee` tablosunu temsil eden model sınıfıdır.

## ☁️ AWS Entegrasyonu Detayı

Proje, `pom.xml` dosyasındaki **`spring-cloud-aws-starter-parameter-store`** bağımlılığını kullanarak veritabanı kimlik bilgilerini yerel konfigürasyon dosyaları yerine AWS'nin güvenli depolama hizmetinden çeker.

```properties
# Load Spring Data Source properties from AWS Parameter Store
spring.config.import=aws-parameterstore:/config/employee-db
```

## 🖼️ Ekran Görüntüsü

AWS ortamında canlıya alınan uygulamanın ekran görüntüsü:

![AWS Screenshot](./screenshot/amazon-web-services-ss-1.PNG)

## 🛠️ Kurulum ve Çalıştırma

### Gerekli Ön Koşullar

* Java Development Kit (JDK 17)

* Maven

* MySQL Veritabanı (`employee_directory` şeması ile)

* AWS hesabı ve Parameter Store'a konfigürasyon (veritabanı bilgileri) erişimi

### 1. Veritabanı Kurulumu

Proje veritabanını oluşturmak ve örnek verileri yüklemek için aşağıdaki SQL komutlarını kullanın (Bu komutlar proje dizininde yer alan `employee-directory-db-setup.sql` dosyasında da mevcuttur):

```sql
DROP USER if exists 'springstudent'@'%' ;

CREATE USER 'springstudent'@'%' IDENTIFIED BY 'springstudent';

GRANT ALL PRIVILEGES ON * . * TO 'springstudent'@'%';

CREATE DATABASE  IF NOT EXISTS `employee_directory`;
USE `employee_directory`;

-- Diğer tablo oluşturma ve veri ekleme komutları...
```

### 2. AWS Parameter Store Konfigürasyonu

AWS Parameter Store'da veritabanı bağlantısı için gerekli parametreleri (**`/config/employee-db`** yolu altında) ayarlayın.

### 3. Projeyi Çalıştırma

```bash
# Projeyi derleyin
mvn clean install

# Uygulamayı başlatın
mvn spring-boot:run
```

Uygulama başladığında otomatik olarak http://localhost:8080/employees/list adresine yönlenecektir.
