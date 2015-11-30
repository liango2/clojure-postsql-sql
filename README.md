# 使用 clojure 连接 postgresal 数据库

## 安装 postgresal

### 创建数据库

d:
cd d:\Program Files\PostgreSQL\9.4\bin\
d:\Program Files\PostgreSQL\9.4\bin>[createdb](http://www.postgresql.org/docs/9.1/static/app-createdb.html) -U postgres lianxi-clojure-sql


### 下载依赖包
```
lein try org.clojure/java.jdbc "0.3.0" java-jdbc/dsl "0.1.0" org.postgresql/postgresql "9.2-1003-jdbc4"     
```


### 定义一个数据库的链接规格说明
```
(def db-spec {:classname   "org.postgresql.Driver"
              :subprotocol "postgresql"
              :subname     "//localhost:5432/lianxi-clojure-sql"
              ;; Not needed for a non-secure local database...
              :user "postgres"
              :password "root"
              })
```


### 建表
```
;; 导包
(require '[clojure.java.jdbc :as jdbc]
         '[java-jdbc.ddl     :as ddl ])


;; 建表,表名:tags
(jdbc/db-do-commands db-spec false
  (ddl/create-table
     :tags
     [:id   :serial    "PRIMARY KEY"]
     [:name :varchar   "NOT NULL"   ]))
;; -> (0)
```

### 插入记录
```
;; 导包
(require '[java-jdbc.sql :as sql])

;; 插入记录
(jdbc/insert! db-spec :tags
                      {:name "Clojure"}
                      {:name "Java"})
;; -> ({:name "Clojure", :id 1} {:name "Java", :id 2})

```
### 查询
```
;; 查询
(jdbc/query db-spec (sql/select * :tags (sql/where {:name "Clojure"})))
;; -> ({:name "Clojure", :id 1})
```


### 参考:

