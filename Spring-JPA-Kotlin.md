### 테스트용 mysql 생성 (docker)
```bash
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

### mysql database 추가
mysql 쉘 접속 (위에서 비밀번호는 paasword 로 설정함)
```bash
docker exec -it mysql bash -c "mysql -p"
```
sql 명령으로 db 추가
```sql
CREATE DATABASE `db` DEFAULT CHARACTER SET utf8mb4;
```

### Entity / Repository 생성  
```kotlin
@Embeddable
data class UserDescriptor(
  var name: String = ""
)

@Entity
class User(
  @Id val id: Long?,
  @Embedded
  val descriptor: UserDescriptor
) {
  constructor() : this(null, UserDescriptor())
}

interface UserRepository : JpaRepository<User, Long>
```

### 실행시 table create / 종료시 drop  
application.properties
```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/db
spring.datasource.username=root
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=create-drop
```

### flyway를 사용한 마이그레이션
src/main/resources/db/migration  
  
V1__<마음대로>.sql 사용시 db 초기화  
V2__<마음대로>.sql 사용시 db 업데이트  


### Form 처리시 사용하는 Annotation
@FormParam("name") value: String


### 파일 다운로드
content-type: application/octet-stream
content-disposition: attachment;filename=<FileName>    (charset 주의)
cache-control: no-cache
  
