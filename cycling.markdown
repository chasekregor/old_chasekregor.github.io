---
layout: page
title: Cycling
permalink: /cycling/
---

In my free time, I really enjoy cycling. 


### stats:

- Age: 26
- Weight: 90.54 kg
- FTP: 214w (2.36 w/kg) @ 5,200 ft

### race results:

### strava segments:
- [Flagstaff climb in Boulder, CO](https://www.strava.com/activities/269398985#6309717255)
- [Lookout mountain in Golden, CO](https://www.strava.com/activities/268323200#6285168866)
- [Chapman climb in Boulder, CO](https://www.strava.com/activities/902550125#66530106057)
- [Mount Lemmon in Tucson, AZ](https://www.strava.com/activities/462549170/segments/11125464275)
- [sa Calobra in Mallorca, Spain](https://www.strava.com/activities/144939589/segments/4976839905)
- [Deer Creek Canyon Climb in Littleton, CO](https://www.strava.com/activities/2058305235#51907769581)

### long rides:
- [Harpeth River Ride](https://www.strava.com/activities/620712853)
- [Phil & Chase’s Country Adventure](https://www.strava.com/activities/1734210949)
- [Jamala Beach Ride](https://www.strava.com/activities/238975609)
- [Jensie Ride](https://www.strava.com/activities/410366821)
- [Hungry Horse Dam](https://www.strava.com/activities/1700634291)
- [Red Rocks & Back From Denver](https://www.strava.com/activities/2455981637)

### cycling related posts:


{% for post in site.categories.Cycling %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
