---
title: "使用Navicat批量导入Excel数据到MySQL数据库"
tags: ["MySQL", "Navicat"]
date: 2018-08-20
---

上线之前经常会遇到初始化数据时SQL整理复杂，除过写脚本还有一种方法就是整理成Excel，直接导入数据库，本文用来介绍使用Navicat批量导入Excel数据到MySQL数据库的步骤

<!--more-->

第一步：在Navicat for MySQL上创建一个表
```SQL
CREATE TABLE "h_resignation_reason" (
  "id" varchar(50) NOT NULL COMMENT 'id',
  "reason_type" varchar(300) DEFAULT NULL COMMENT '原因类型',
  "resignation_reason" varchar(300) DEFAULT NULL COMMENT '离职原因',
  "resignation_type" int(1) DEFAULT NULL COMMENT '离职类型',
  "status" int(1) DEFAULT NULL COMMENT '状态',
  "update_user" varchar(50) DEFAULT NULL COMMENT '修改人',
  "update_time" datetime DEFAULT NULL COMMENT '修改时间',
  "create_user" varchar(50) DEFAULT NULL COMMENT '创建人',
  "create_time" datetime DEFAULT NULL COMMENT '创建时间',
  PRIMARY KEY ("id")
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='离职原因配置表';
```
第二步：打开excel表,按照程序提供的字段填写相应的数据

注意红色框中的内容需要和数据库中的字段名一致

黄色框中的内容可以删去，不需要存在，打码部分为待传入数据

![image](/media/posts/import-excel-data-into-MySQL-database/1.png)

第三步：使用的mysql管理工具Navicat for MySQL，打开工具，选择表所在的数据库，然后点击数据库名字，点击“Import Wizard”，弹出如下选择界面，我们选择“Excel file文件(2007或以上版本)(*.xlsx)”，点击“下一步”

![image](/media/posts/import-excel-data-into-MySQL-database/2.png)

第四步：紧接着选择刚才新建的Excel文件就行,然后再下面选文件内容在哪一个sheet中，一般都是第一个，若有修改这里需要注意，点击“Next”

![image](/media/posts/import-excel-data-into-MySQL-database/3.png)

第五步：(此步骤也是关键步骤)，需要注意2点: 

1. filed name row：就是字段名所在Excel中的位置(也就是图一中的红框内容)
2. first data row：数据从哪一行开始(也就是图一中的打码内容第一行)

![image](/media/posts/import-excel-data-into-MySQL-database/4.png)

第六步：选择“目标表”，对应数据库中的表名，可以下拉选择要导入到哪个表中，选好后点击“下一步”

![image](/media/posts/import-excel-data-into-MySQL-database/5.png)

第七步：可以看到Excel表格和数据库中的已经一一对应无误，点击下一步

![image](/media/posts/import-excel-data-into-MySQL-database/6.png)

第八步：根据业务需求选择一个需要的导入模式，选择好后点击“下一步”

![image](/media/posts/import-excel-data-into-MySQL-database/7.png)

第九步：点击“start”修改数据库数据，出现如下结果表示修改成功，点击“close”关闭弹框

![image](/media/posts/import-excel-data-into-MySQL-database/8.png)

接下来打开对应数据表就会看到新导入的数据