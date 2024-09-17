## Mybatis

mybatis是一个半自动化的ORM框架

### ORM
ORM是object relationshiop mapping的缩写，指将java中的bean映射到数据库重的表

### mybatis核心对象
* SqlSessionFactory
* SqlSession
* Mapper

### 延迟加载
1.编码的时候通过xml里面的association标签实现延迟加载
2.在配置里面通过以下代码开启延迟加载
```
<settings>
    <setting name="lazyLoadingEnabled" value="true"/>
     <setting name="aggressiveLazyLoading" value="false"></setting>
</settings>
```

### 缓存
缓存分为一级缓存和二级缓存
1.一级缓存
    在sqlSession里面缓存，默认是开启的，关闭sqlSession后或者执行dml操作后清空缓存

2.二级缓存
    在mapper级别缓存，执行dml操作后清理缓存，需要根据以下配置开启
```
<settings>
    <setting name="cacheEnabled" value="true"/>默认是false：关闭二级缓存
<settings>
```

### #{}和${}
#{}是预编译处理，可以防止sql注入，${}是直接字符替换，如果参数是内部编码而非外部参数传入的时候可以用字符替换

### 编程式事务
```
SqlSession sqlSession = sqlSessionFactory.openSession(false);
try {
    // 执行数据库操作
    sqlSession.insert("insertUser", user);
    sqlSession.update("updateUser", user);
    
    // 提交事务
    sqlSession.commit();
} catch (Exception e) {
    // 回滚事务
    sqlSession.rollback();
} finally {
    // 关闭 SqlSession
    sqlSession.close();
}
```

### 声明式事务
需要配合spring然后写一堆配置