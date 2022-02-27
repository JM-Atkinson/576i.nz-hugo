---
title: "World War II in film"
date: 2022-02-13T01:26:38+13:00
type: article
draft: false
tags: [ "film", "history" ]
---

{{< figure src="inthiscorneroftheworld2.jpg" caption="" attr="In This Corner of the World (2016)" attrlink="" >}}

When I think of the genres of film that I consider myself an ‘extra’ fan of, films about war is one of the genres that often comes near the top of the list. War is something that has profound geopolitical, social, and physical ramifications for all involved, and often lasting side effects that will cause sadness and pain for decades to come. The opportunity to watch the stories that come of it, to learn and respect those who went before, is something I value being able to do.

World War II, as the most significant event to happen to society at large perhaps ever, had gigantic geopolitical effects that nearly all conflict in the years since can draw lines to establishing the puzzle pieces. It engulfed smaller (in comparison to the entire globe) regional conflicts such as the Second Sino-Japanese War, fundamentally altering it's course and eventually resulting in the foundation of modern communist China. So many aspects of our world today are directly attributable to the events of World War II.

World War II is not like other conflicts in that the events are easily geographically definable to a specific region. Being the only truly global conflict, events were spread the world over, and as such it can be hard to picture the extent of all involved. However, film is a way to do this, and we see snapshots of particular environments, and the conditions that contended the people there. So much film about World War II has been made, that one can start to build a picture of the entire war if they endeavoured to watch it all. Chances are, if there's a World War II battle you've heard of, there's a film about it.

Several years ago, I thought an exercise could be done then to plot all the films I've seen about World War II on a map, and depict my experience of World War II through film, geographically. Only since I began writing this site, have I gained the skills to make this a reality. So I began a project to product my ‘World War II film map’, and the results are here for you to view.

To view the map, you can [**click here**](/wwiifilmmap), or go back to the articles page and choose the interactive version of this post.

{{< figure src="schindler.jpg" caption="" attr="Schindler's List (1993)" attrlink="" >}}

#### Viewer's guide

The scope of this map is limited to films & miniseries about World War II, and it's sub-conflicts that happened immediately before and after the main conflict.

Due to practicality, most films depicted have a point placed roughly at the centre of the geographic area they are set, and the specificity of this varies enormously depending on the scope and realism of the film. Sometimes we know even down to the street corner where a scene takes place, sometimes we know little more than a country. When possible I have tried to be as accurate as I can.

In scenarios where a film takes place over several locations, I will include multiple points for this film for each location. Again for practicality, I've not included locations that were depicted for less than about 15 minutes of screen runtime, or where there would be many points due to the film being primarily travelling (Saving Private Ryan as an example).

{{< figure src="tora.jpg" caption="" attr="Tora! Tora! Tora! (1970)" attrlink="" >}}

#### Technology Colophon

This entire project was surprisingly simple to construct. I've taken advantage of several toolsets to make this easier for me.

Firstly, the main map itself is provided by [Leaflet](https://leafletjs.com/), one of the best JS map libraries going. The base layer map is provided by the awesome [OpenStreetMap](https://www.openstreetmap.org/) project (which is free to use!).

To plot the points on the map, I sought out a simple data structure that I could modify using a easy GIS tool to plot my points. I investigated a number of geographical formats, and ways to use them with Leaflet, until I came across [GeoJSON](https://geojson.org/), which is really the hero of the show here. GeoJSON is a standardised JSON formatted way to store geographical objects, like points, polygons, and lines. [Leaflet natively supports GeoJSON](https://leafletjs.com/examples/geojson/), which made this whole exercise as easy is fetching a GeoJSON feature set, and feeding that into Leaflet and an input.

I can build my GeoJSON files using anything that produces GeoJSON including most GIS tools, but an easy solution is [geojson.io](http://geojson.io/).

To help people understand what film corresponds to a point, I had originally planned to write descriptions about each point. However, the smarter way to do this is to work with a community film database like [TMDB](https://www.themoviedb.org/), and query their wonderful data. This means my popups won't go out of date, and makes my job easier as I don't have to copy-paste hundreds of lines of text. I simply include the TMDB ID in the GeoJSON feature, and this is then queried against TMDB's API, which gives me all my details. This is how the pop-ups are populated.

And that's pretty much it!

{{< figure src="savingprivateryan.jpg" caption="" attr="Saving Private Ryan (1998)" attrlink="" >}}

#### A word on the purpose of war films

I am cautious of the fact that when writing about my ‘love’ for war films I could be accused of romanticising conflict. I think this takes a dim view on the purpose of what most film about war is. Some early film about war could indeed be argued to romanticise the events they depict, in some cases essentially propaganda. But on the whole, the tone of war films has generally shifted to be an anti-war one, and critical reception has co-opted this. Film about war serves to showcase to later generations what life in this global conflict was like. It enables us to understand the fear, suffering, pain, and destruction that was unleashed on the world, and be shaken to our cores. We get a bridge of understanding to their world, and that for me is fundamentally the point of film.

The theatres of World War II saw unparalleled variation of environments, from frigid room-to-room combat in the siege of Leningrad, to tropical dogfights above the largest naval fleets since seen. And as such, the soldiers and innocent civilians that were involved in these environments generated millions of stories, only a tiny fraction of which we have the luck to be able to listen to today.

By depicting these stories in film, we can better understand what our ancestors went through, grit and all. I don't know about other people, but through watching so many angles of World War II, not ever have I come away wanting to physically have been there, instead humbled by the incredible & tragic stories I experienced.