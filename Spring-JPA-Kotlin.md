### 테스트용 mysql 생성 (docker)
```bash
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

### mysql database 추가
mysql 쉘 접속 
```bash
docker exec -it mysql bash -c "mysql -p"
```
sql 명령으로 db 추가
```sql
CREATE SCHEMA `db` DEFAULT CHARACTER SET utf8mb4;
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
