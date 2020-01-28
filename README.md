![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Find All Full Text Indexes With SQL
**Post Date: June 16, 2014**





## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Want to find all the Full Text Indexes across every database? You could do one at a time, but that's annoying, and there's always a better way of doing it.
So… to get them all try this little piece of sql logic:</p>

## SQL-Logic
```SQL
use master;
set nocount on
declare @get_all_full_text_catalogs varchar(max)
set @get_all_full_text_catalogs = ''
select @get_all_full_text_catalogs = @get_all_full_text_catalogs + '
use [' + name + '];' + char(10) +
'
select
''DatabaseName'' = db_name()
, ''TableName'' = st.name
, ''FTCatalogName'' = sfc.name
, ''FileGroupName'' = sf.name
, ''IndexName'' = si.name
, ''ColumnName'' = sc.name
from
sys.tables st
join sys.fulltext_indexes sfti on st.[object_id] = sfti.[object_id]
join sys.fulltext_index_columns sftic on sftic.[object_id] = st.[object_id]
join sys.columns sc on sftic.column_id = sc.column_id and sftic.[object_id] = sc.[object_id]
join sys.fulltext_catalogs sfc on sfti.fulltext_catalog_id = sfc.fulltext_catalog_id
join sys.filegroups sf on sfti.data_space_id = sf.data_space_id
join sys.indexes si on sfti.unique_index_id = si.index_id and sfti.[object_id] = si.[object_id]; ' + char(10)
from sys.databases where name not in ('master', 'model', 'msdb', 'tempdb') order by name asc

exec (@get_all_full_text_catalogs) --for xml path(''), type
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)


## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")


