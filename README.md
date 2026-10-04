# Docker Compose 실행

프로젝트 루트의 `docker-compose.yml`에서 프런트엔드와 백엔드 이미지를 빌드하고 MySQL을 실행합니다. 프런트엔드와 백엔드는 별도 Git 저장소이므로, Compose 파일을 받은 뒤 같은 폴더에 각각 체크아웃해야 합니다. 데이터 파이프라인 저장소는 Compose 실행에 필요하지 않습니다.

```text
catlog/                       # docker-compose.yml, README.docker.md
├─ frontend/                  # https://github.com/hepatica387/catlog.git
└─ backend/                   # https://github.com/hepatica387/catlog_backend.git
   └─ sql/schema.sql          # MySQL 최초 초기화에 사용
```

`docker-compose.yml`이 있는 폴더에서 프로젝트를 받습니다. 이미 두 폴더가 있다면 다시 받을 필요가 없습니다.

```powershell
git clone https://github.com/hepatica387/catlog.git frontend
git clone https://github.com/hepatica387/catlog_backend.git backend
docker compose up --build -d
```

Compose의 빌드 경로는 `./frontend`, `./backend`이고, SQL 파일 경로는 `./backend/sql/schema.sql`입니다. 폴더 이름이나 위치가 다르면 `docker-compose.yml`의 경로도 함께 수정해야 합니다.


- 프런트엔드: http://localhost:3000
- 백엔드: http://localhost:8080
- MySQL: localhost:3308 (DB `catlog`, 사용자 `catlog`)

로컬 개발용 기본 비밀번호가 설정되어 있습니다. 변경하려면 프로젝트 루트에 `.env` 파일을 만들고 `MYSQL_PASSWORD`, `MYSQL_ROOT_PASSWORD`를 지정하세요. 포트는 `FRONTEND_PORT`, `BACKEND_PORT`, `MYSQL_PORT`로 바꿀 수 있습니다.

MySQL 데이터는 `mysql_data` 볼륨에 저장됩니다. `backend/sql/schema.sql`은 새 볼륨을 처음 생성할 때만 실행됩니다. 종료는 `docker compose down`을 사용합니다.
