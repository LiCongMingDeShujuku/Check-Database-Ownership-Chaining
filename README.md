![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 检查数据库的所有权链接
#### Check Database Ownership Chaining
**发布-日期: 2017年11月2日 (评论)**

![#](images/check-for-sql-database-ownership-chaining.png?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
运行此SQL Logic以确定数据库所有权链接是否被启用。它会检查引擎配置和每个数据
库。请记住，某些系统数据库不能使用配置集，因此我排除了某些系统数据库的id。这完全正常。
还可以让你少一些点击。

## English
Run this SQL Logic to determine if database ownership chaining is enabled. It checks both the engine configuration, and each database. Remember; some system databases cannot have the configuration set so I have excluded database_id’s of certain system databases. This is completely normal.
This should save you some keystrokes.

---
## Logic
```SQL

use [master];
set nocount on
 
	--check database chaining engine wide
	--在引擎范围内检查数据库链接
select
    'sql_instance'  = @@servername
,   'configuration' = upper([name])
,   'setting'   	= case when [value_in_use] = '1' then 'ON' else 'OFF' end from sys.configurations where [name] = 'cross db ownership chaining'
 
	--disable ownership chaining for engine
	--禁用引擎的所有权链接
execute sp_configure 'show advanced', 1;  				reconfigure;  
execute sp_configure 'cross db ownership chaining', 0;  reconfigure;  
 
	--disable ownership chaining per database
	--禁用每个数据库的所有权链接
declare @set_db_chaining_off    varchar(max)
set @set_db_chaining_off    	= ''
select  @set_db_chaining_off    = @set_db_chaining_off + 
'alter database [' + name + '] set db_chaining off;' + char(10)
from    sys.databases where [database_id] > 3
exec    (@set_db_chaining_off)
 
	--check database chaining per database
	--sdf检查每个数据库的数据库链接
select
    'database'  = upper([name])
,   'db_chaining'   = case when [is_db_chaining_on] = '0' then 'Off' else 'On' end
from    sys.databases where [database_id] > 3 order by [is_db_chaining_on] desc

```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

