表结构如下:

  ```
  CREATE TABLE `employees` (
    `id` int(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'id',
    `name` varchar(64) NOT NULL COMMENT '雇员姓名',
    `age` TINYINT(5) NOT NULL COMMENT '雇员年龄',
    `addr` varchar(64) NOT NULL COMMENT '雇员家庭地址',
     PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COMMENT='雇员信息表';
  ```


```
type Employee struct {
	Name string `gorm:"column:name"`
	Age  int    `gorm:"column:age"`
	Addr string `gorm:"column:addr"`
}
// BatchSave 批量插入数据
func BatchSave(db *gorm.DB, emps []*Employee) error {
	var buffer bytes.Buffer
	sql := "insert into `employees` (`name`,`age`,`addr`) values"
	if _, err := buffer.WriteString(sql); err != nil {
		return err
	}
	for i, e := range emps {
		if i == len(emps)-1 {
			buffer.WriteString(fmt.Sprintf("('%s','%d',%s);", e.Name, e.Age, e.Addr))
		} else {
			buffer.WriteString(fmt.Sprintf("('%s','%d',%s),", e.Name, e.Age, e.Addr))
		}
	}
	return db.Exec(buffer.String()).Error
}
```
