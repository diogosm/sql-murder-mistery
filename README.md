# sql-murder-mistery

## my solution steps

### crime scene info

Get data from crime scene investigation

    select * from crime_scene_report
    where date = '20180115'
    and city = 'SQL City'
    and type = 'murder';
    -- Security footage shows that there were 2 witnesses. The first witness lives 
    --at the last house on "Northwestern Dr". The second witness, named Annabel, 
    --lives somewhere on "Franklin Ave".

In this point, I got some data from interviews. It was easy to relate the report given to the description of murder using something like this `select * from interview where person_id = 67318;`

### Get data from person related to the crime scene

    select * 
    from person p1
    left join income i1 on p1.ssn = i1.ssn
    where (p1.address_street_name like '%Northwestern Dr%')
    union
    select * 
    from person p2
    left join income i2 on p2.ssn = i2.ssn
    where (p2.name like '%Annabel%' and 
    	  p2.address_street_name like '%Franklin%')

### Get data from murderer

That data was based on the interviewers

    select * from person
    where license_id in (
    select id
    from drivers_license
    where id in (
      select license_id
      from person
      where id in (28819,67318)
      ))

### The criminal mind

To get the mind behind the crime, again, I applied a sql in table `interview`. After that, I've made the following sequence of SQLs:

    select *
    from facebook_event_checkin
    where person_id in (
	    select id --*
	    from person
	    where license_id in (
		    select id
		    from drivers_license
		    where car_make like '%Tesla%'
		    and car_model like '%Model S%'
		    and hair_color = 'red'
		    and gender = 'female')
    )

Since only one id is found (try `distinct` in the SQL above), you can just apply the SQL below with the id found.

    select *
    from person
    where license_id in (
    select id
    from drivers_license
    where car_make like '%Tesla%'
    and car_model like '%Model S%'
    and hair_color = 'red'
    and gender = 'female')

## Result

    INSERT INTO solution VALUES (1, 'Miranda Priestly');
            
            SELECT value FROM solution;
