该部分不借助代码生成，完成单/多表及自定义业务功能的编码实现

以下只是做了一个简单的多表示例，将b_crud的bname字段内容修改为element_\_definition表的记录数（数字转字母）

1 建表（同前，略）

2 编写Model文件 BCrud.java

```
...
public class BCrud extends BaseEntity <Long>
{
  private String bname ;

  private Date bdate;

  public String getBname ()
  {
  return bname;
  }

  public void setBname(Stringbname)
  {
  this.bname=bname;
  }

  public Date getBdate()
  {
  return bdate ;
  }


  public void setBdate (Date bdate)
  {
   this.bdate=bdate;
  }

}
```

 3 编写Dao文件 MultiTableDao.java

```
...
@Mapper
public interface MultiTableDao
{
  int updatecount();
}
```

 4 编写mapper文件 MultiTableMapper.xml

如果不懂怎么编写mapper文件 ，参考书籍 《Mybatis从入门到精通》或其他mybatis的书

```
<?xml version ="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper PUBLIC"-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace ="com.boot.security.server.dao.MultiTableDao">

<update id="updatecount">
update b_crud set bname =CONCAT((select count (1) from element_definition),'')
</update>

</mapper>
```

 

5 编写service文件 MultiTableService.java

```
package com.boot.security.server.service;

public interface MultiTableService
{
 void changebcrubcount();
}
```

6 实现MultiTableServiceImpl.java

```
public class MultiTableServiceImpl implements MultiTableService
{

private static final Logger log=LoggerFactory.getLogger("adminLogger");

@Autowired
private MultiTableDao multiTableDao ;

@Override
public void changebcrubcount()
{
 multiTableDao.updatecount();

 log.info("测试下日志 from changebcrubcount()");
}
```

 7 编写controller文件 MultiTableController.java

```
@RestController
@RequestMapping("/multitable")
public class MultiTableController
{

@Autowired
private MultiTableService multiTableService;

@PostMapping("/updatecount")
public int updatecount()
{
    multiTableService.changebcrubcount();

    return 1;
}

}
```

 8 修改config/SecurityConfig.java\(为了暂时屏蔽token的过滤，方便下一步的接口调试\)

```
...

antMatchers("/","/*.html","/favicon.ico","/css/**","/js/**"
,"/fonts/**","/layui/**","/img/**","/v2/api-docs/**",
"/swagger-resources/**","/webjars/**","/pages/**",
"/druid/**","/statics/**","/multitable/**"
).permitAll().anyRequest().authenticated()
;

...
```

在最后加上“/multitable/\*\*",表明这个路径的请求不使用springsecurity进行控制

 9 swagger页面进行接口调试

选“接口swagger”，选中multi-table-contrllor，点击请求按钮进行测试

![](/assets/屏幕快照 2018-05-23 下午12.51.56.png)try it out  -&gt; Execute

返回值如图，说明调用成功

![](/assets/屏幕快照 2018-05-23 下午12.55.20.png)查询数据库表可见数据表对应列数值已按要求改变

10 新增方法，尝试带参数 updatecountbyid\(Long id\),只更新对应id的bname字段，测试方法同前

```
controller:
...
@PostMapping("/updatecount/{id}")
public int updatecountById(@PathVariable Long id){
  multiTableService.changebcrudcountById(id);
return 1;
}
...

Dao:
...
int updatecountbyid(Long id);
...

mapper:
...
<update id="updatecountbyid" >
update b_crud set bname = CONCAT((select count(1) from element_definition),'') where id=#{id}
</update>
...

service:
...
void changebcrudcountById(Long id);
...

serviceimpl:
...
@Override
public void changebcrudcountById(Long id) {
  multiTableDao.updatecountbyid(id);

  log.info("测试下日志 from changebcrubcountbyid()");
}
...
```

11 新增实体类参数，并根据实体类的属性值进行更新 类似于saveBcrud（Bcrud bcrud）这样的方式

具体步骤可参考其他controllor中对应的方法进行改造，具体步骤略

12 改回config/SecurityConfig中的配置

