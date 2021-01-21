### SQL script
```sql
select * from airport_freq 
where airport_ident = 'KLAX' 
order by type desc 
```

### Python code
```python
def my_func(a, b):
    return a + b
```

# SQL and Pandas cheatsheet.

<table>
  
<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airports
   </pre>
</td>
<td>
  <pre lang="python">
airports  
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airports 
limit 3
   </pre>
</td>
<td>
  <pre lang="python">
airports.head(3)
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select id from airports 
where ident = 'KLAX'
   </pre>
</td>
<td>
  <pre lang="python">
airports[airports.ident == 'KLAX'].id
   </pre>
</td>
</tr>


<tr>
<td>
  <pre lang="text">
SELECT
DISTINCT
   </pre>
</td>
<td>
  <pre lang="sql">
select distinct type 
from airport
   </pre>
</td>
<td>
  <pre lang="python">
airports.type.unique()
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airports 
where iso_region = 'US-CA' 
  and type = 'seaplane_base'
   </pre>
</td>
<td>
  <pre lang="python">
airports[(airports.iso_region == 'US-CA') 
        & (airports.type == 'seaplane_base')]
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select ident, name, municipality 
from airports 
where iso_region = 'US-CA' 
  and type = 'large_airport'
   </pre>
</td>
<td>
  <pre lang="python">
airports[(airports.iso_region == 'US-CA') 
         & (airports.type == 'large_airport')]
       [['ident', 'name', 'municipality']]
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airport_freq 
where airport_ident = 'KLAX' 
order by type
   </pre>
</td>
<td>
  <pre lang="python">
airport_freq[airport_freq.airport_ident == 'KLAX']
           .sort_values('type')
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airport_freq 
where airport_ident = 'KLAX' 
order by type desc
   </pre>
</td>
<td>
  <pre lang="python">
airport_freq[airport_freq.airport_ident == 'KLAX']
           .sort_values('type', ascending=False)
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airports 
where type in ('heliport', 'balloonport')
   </pre>
</td>
<td>
  <pre lang="python">
airports[airports.type.isin(['heliport', 'balloonport'])]
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select * 
from airports 
where type not in ('heliport', 'balloonport')
   </pre>
</td>
<td>
  <pre lang="python">
airports[~airports.type.isin(['heliport', 'balloonport'])]
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select iso_country, type, count(*) 
from airports 
group by iso_country, type 
order by iso_country, type
   </pre>
</td>
<td>
  <pre lang="python">
airports.groupby(['iso_country', 'type'])
        .size()
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select iso_country, type, count(*) 
from airports 
group by iso_country, type 
order by iso_country, count(*) desc
   </pre>
</td>
<td>
  <pre lang="python">
airports.groupby(['iso_country', 'type'])
        .size()
        .to_frame('size')
        .reset_index()
        .sort_values(['iso_country', 'size'], 
                     ascending=[True, False])
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select type, count(*) 
from airports 
where iso_country = 'US' 
group by type 
having count(*) > 1000 
order by count(*) desc
   </pre>
</td>
<td>
  <pre lang="python">
airports[airports.iso_country == 'US']
       .groupby('type')
       .filter(lambda g: len(g) > 1000)
       .groupby('type')
       .size()
       .sort_values(ascending=False)
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select iso_country 
from by_country 
order by size desc 
limit 10
   </pre>
</td>
<td>
  <pre lang="python">
by_country.nlargest(10, columns='airport_count')
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select iso_country 
from by_country 
order by size desc 
limit 10 offset 10
   </pre>
</td>
<td>
  <pre lang="python">
by_country.nlargest(20, columns='airport_count')
          .tail(10)
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select max(length_ft), min(length_ft), 
       avg(length_ft), median(length_ft) 
from runways
   </pre>
</td>
<td>
  <pre lang="python">
runways.agg({'length_ft': ['min', 'max', 'mean', 'median']})
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select airport_ident, type, description, frequency_mhz 
from airport_freq inner join 
     airports on airport_freq.airport_ref = airports.id 
where airports.ident = 'KLAX'
   </pre>
</td>
<td>
  <pre lang="python">
airport_freq.merge(airports[airports.ident == 'KLAX'][['id']], 
                  left_on='airport_ref', right_on='id', how='inner')
            [['airport_ident', 'type', 'description', 'frequency_mhz']]
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
select name, municipality 
from airports 
where ident = 'KLAX' 
union all 
select name, municipality 
from airports 
where ident = 'KLGB'
   </pre>
</td>
<td>
  <pre lang="python">
pd.concat([
           airports[airports.ident == 'KLAX'][['name', 'municipality']], 
           airports[airports.ident == 'KLGB'][['name', 'municipality']]
         ])
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
create table heroes (id integer, name text);
insert into heroes 
values (1, 'Harry Potter');
insert into heroes 
values (2, 'Ron Weasley');
   </pre>
</td>
<td>
  <pre lang="python">
df1 = pd.DataFrame({'id': [1], 'name': ['Harry Potter']})
df2 = pd.DataFrame({'id': [2], 'name': ['Ron Weasley']})
df = pd.concat([df1, df2]).reset_index(drop=True)
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
update airports 
set home_link = 'http://www.google.com'
   </pre>
</td>
<td>
  <pre lang="python">
airports.loc['home_link'] = 'http://www.google.com'
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
update airports 
set home_link = 'http://www.lawa.org/welcomelax.aspx' 
where ident == 'KLAX'
   </pre>
</td>
<td>
  <pre lang="python">
airports.loc[
             airports['ident'] == 'KLAX', 
             'home_link'
            ] = 'http://www.lawa.org/welcomelax.aspx'
   </pre>
</td>
</tr>

<tr>
<td>
  <pre lang="text">
SELECT
   </pre>
</td>
<td>
  <pre lang="sql">
delete from lax_freq 
where type = 'MISC'
   </pre>
</td>
<td>
  <pre lang="python">
lax_freq = lax_freq[lax_freq.type != 'MISC']
#OR
lax_freq.drop(lax_freq[lax_freq.type == 'MISC'].index)
   </pre>
</td>
</tr>

  
</table>
