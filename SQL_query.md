### SQL query

```.py
Nagisa --- select city from crime_scene_report where type = 'murder' order by city asc;
Reiji --- select count(car_make) from drivers_license where car_make = 'Mercedes-Benz';
Bee --- select address_street_name as Frequency from person group by address_street_name order by count(address_street_name) desc;
        select count(address_street_name) as Frequency from person group by address_street_name order by count(address_street_name) desc;
        select address_street_name from person where address_street_name ='Northwestern Dr';
Elia --- select count(type) from crime_scene_report where type = 'theft';
Kojiro --- select count(date) from facebook_event_checkin where 20170700 <= date < 20170800;
David --- select membership_status, annual_income from person inner join income i on person.ssn = i.ssn inner join get_fit_now_member g on person.name = g.name order by annual_income asc;
Michael --- select count(date) from crime_scene_report group by substr(cast(date as text), 5, 2) order by substr(cast(date as text), 5, 2) desc;
Aup --- select count(description) from crime_scene_report where description like 'nudism'
```
