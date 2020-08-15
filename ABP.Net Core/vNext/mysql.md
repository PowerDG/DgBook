

# 一、ABP Vnext后端运行+切mysql数据库







abp.io网站选择angular、EF、不要身份单独服务器起个名字下载下来项目。



![img](https:////upload-images.jianshu.io/upload_images/16547157-21ebfe92e97502b6.png?imageMogr2/auto-orient/strip|imageView2/2/w/470)

项目截图

1、第一步修改为mysql数据库，这一步是因为我目前项目用的mysql，要是sql的话就没这么麻烦事了，什么切数据库，字段长短限制各种恶心人。

EntityFrameworkCore 下安装包：Volo.Abp.EntityFrameworkCore.MySQL



删除Volo.Abp.EntityFrameworkCore.sqlserver



![img](https:////upload-images.jianshu.io/upload_images/16547157-a652d3c8353f83c9.png?imageMogr2/auto-orient/strip|imageView2/2/w/443)

包

2、EntityFrameworkCoreModule

using Volo.Abp.EntityFrameworkCore.SqlServer 改为 using Volo.Abp.EntityFrameworkCore.MySQL;

typeof(AbpEntityFrameworkCoreSqlServerModule)改为  typeof(AbpEntityFrameworkCoreMySQLModule),

 options.UseSqlServer(); 改为 options.UseMySQL();



![img](https:////upload-images.jianshu.io/upload_images/16547157-c2e7e05e8464e4d5.png?imageMogr2/auto-orient/strip|imageView2/2/w/769)

修改点

3、编译后会发现MigrationsDbContextFactory报错

 .UseSqlServer(configuration.GetConnectionString("Default"));改为    .UseMySql(configuration.GetConnectionString("Default"));



4、JunTuanAlliance.EntityFrameworkCore.DbMigrations文件下的Migrations文件夹整体删掉一会命令重新生成。

重新编译，全部成功。

5、修改数据库连接字符串

JunTuanAlliance.DbMigrator下appsettings.json



![img](https:////upload-images.jianshu.io/upload_images/16547157-48b0dd0ddf4760fa.png?imageMogr2/auto-orient/strip|imageView2/2/w/1062)

数据库连接字符

拷贝一份appsettings.json到JunTuanAlliance.EntityFrameworkCore.DbMigrations下，一会数据迁移的时候使用。

JunTuanAlliance.HttpApi.Host下也有一份，也需要修改连接字符。



6、JunTuanAlliance.EntityFrameworkCore.DbMigrations设置为启动项目。

打开程序包管理控制台



![img](https:////upload-images.jianshu.io/upload_images/16547157-d804f4a40c3578f0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200)

打开路径



默认项目选择JunTuanAlliance.EntityFrameworkCore.DbMigrations

输入命令 Add-Migration -Context JunTuanAllianceMigrationsDbContext

输入生成的文件名字 

回车



![img](https:////upload-images.jianshu.io/upload_images/16547157-feec051b13880bbc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200)

结果图



因为mysql有最大字符的限制，4000，2000这种的我改为245，之后不够用再说吧，懒得再找方法调高了。

不改的话会报错BLOB/TEXT column 'Value' used in key specification without a key length。

然后执行：Update-Database -Context JunTuanAllianceMigrationsDbContext，创建OK。

7、启动项目改为JunTuanAlliance.DbMigrator并运行，生成默认数据。

8、然后启动项目改为JunTuanAlliance.HttpApi.Host后运行。



![img](https:////upload-images.jianshu.io/upload_images/16547157-e0bbe2b9a1f7dc84.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200)

结果图

后端的目前先到此结束。



作者：灰纸黑字
链接：https://www.jianshu.com/p/b9afeeed3cd9
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。