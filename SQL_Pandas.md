# OK
## OK
### OK

|                                                                     SQL                                                                     |                                                                   Pandas                                                                   |
|:-------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------:|
| select * from airports                       | airports                              |
| select * from airports limit 3               | airports.head(3)                      |
| select id from airports where ident = 'KLAX' | airports[airports.ident == 'KLAX'].id |
| select distinct type from airport            | airports.type.unique()                |
| select * from airports where iso_region = 'US-CA' and type = 'seaplane_base'                         | airports[(airports.iso_region == 'US-CA') & (airports.type == 'seaplane_base')]                                    |
| select ident, name, municipality from airports where iso_region = 'US-CA' and type = 'large_airport' | airports[(airports.iso_region == 'US-CA') & (airports.type == 'large_airport')][['ident', 'name', 'municipality']] |
| select * from airport_freq where airport_ident = 'KLAX' order by type      | airport_freq[airport_freq.airport_ident == 'KLAX'].sort_values('type')                  |
| select * from airport_freq where airport_ident = 'KLAX' order by type desc | airport_freq[airport_freq.airport_ident == 'KLAX'].sort_values('type', ascending=False) |
| select * from airports where type in ('heliport', 'balloonport')     | airports[airports.type.isin(['heliport', 'balloonport'])]  |
| select * from airports where type not in ('heliport', 'balloonport') | airports[~airports.type.isin(['heliport', 'balloonport'])] |
| select iso_country, type, count(&ast;) from airports group by iso_country, type order by iso_country, type              | airports.groupby(['iso_country', 'type']).size()                                                                                              |
| select iso_country, type, count(&ast;) from airports group by iso_country, type order by iso_country, count(&ast;) desc | airports.groupby(['iso_country', 'type']).size().to_frame('size').reset_index().sort_values(['iso_country', 'size'], ascending=[True, False]) |
| select type, count(&ast;) from airports where iso_country = 'US' group by type having count(&ast;) > 1000 order by count(&ast;) desc | airports[airports.iso_country == 'US'].groupby('type').filter(lambda g: len(g) > 1000).groupby('type').size().sort_values(ascending=False) |
| select iso_country from by_country order by size desc limit 10           | by_country.nlargest(10, columns='airport_count')          |
| select iso_country from by_country order by size desc limit 10 offset 10 | by_country.nlargest(20, columns='airport_count').tail(10) |
| select max(length_ft), min(length_ft), avg(length_ft), median(length_ft) from runways | runways.agg({'length_ft': ['min', 'max', 'mean', 'median']}) |
| select airport_ident, type, description, frequency_mhz from airport_freq join airports on airport_freq.airport_ref = airports.id where airports.ident = 'KLAX' | airport_freq.merge(airports[airports.ident == 'KLAX'][['id']], left_on='airport_ref', right_on='id', how='inner')[['airport_ident', 'type', 'description', 'frequency_mhz']] |
| select name, municipality from airports where ident = 'KLAX' union all select name, municipality from airports where ident = 'KLGB' | pd.concat([airports[airports.ident == 'KLAX'][['name', 'municipality']], airports[airports.ident == 'KLGB'][['name', 'municipality']]]) |
| create table heroes (id integer, name text);       | df1 = pd.DataFrame({'id': [1, 2], 'name': ['Harry Potter', 'Ron Weasley']}) |
| insert into heroes values (1, 'Harry Potter');     | df2 = pd.DataFrame({'id': [3], 'name': ['Hermione Granger']})               |
| insert into heroes values (2, 'Ron Weasley');      |                                                                             |
| insert into heroes values (3, 'Hermione Granger'); | pd.concat([df1, df2]).reset_index(drop=True)                                |
| update airports set home_link = 'http://www.lawa.org/welcomelax.aspx' where ident == 'KLAX' | airports.loc[airports['ident'] == 'KLAX', 'home_link'] = 'http://www.lawa.org/welcomelax.aspx' |
| update airports set home_link = 'http://www.lawa.org/welcomelax.aspx' | airports['home_link'] = 'http://www.google.com' |
| delete from lax_freq where type = 'MISC' | lax_freq = lax_freq[lax_freq.type != 'MISC']           |
|                                          | lax_freq.drop(lax_freq[lax_freq.type == 'MISC'].index) |
