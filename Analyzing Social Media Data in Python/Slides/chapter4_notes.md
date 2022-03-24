## Maps and twitter data

### 1. Maps and Twitter data

In this chapter, we're going to working with the geographical Twitter data. We're going to learn how to extract geographical coordinates from the Twitter JSON and how to put that data onto maps in Python. We're also going to learn about the Basemap package and how to create maps in Basemap for use with Twitter data and beyond.

### 2. Why maps?

Why should we want to map Twitter data? There's a number of reasons we'd want to put Twitter data onto a map. First, we want to understand the geographical concentration of our Twitter dataset. If we have a dataset of tweets about an event, are people tweeting participants in the event or are they observers commenting on the event or the news coverage? We can also use mapping as a means to differentiate between different types of tweets and Twitter users. For instance, do data scientists tweet about R or Python more in Bangalore? Or do NBA fans tweet more for the Warriors or the Lakers more in California?

### 3. How Twitter gets location data

Twitter obtains location data in a number of ways. Not all tweets contain location data. Users have to opt in to share their data. From there, their location is obtained from their device, whether it's a smart phone, tablet, or laptop. The accuracy of these locations can vary widely between devices, so in general, it makes more sense in practice to aggregate locations up to the county or state level.

### 4. Beware selection biases!

A word of warning -- only a very small fraction of Twitter data contains any kind of location data. Given that Twitter users need to opt-in to having their location data collected, and that not all devices have this capability, only about 1-3% percent of all Twitter data have any location data associated with it. This limits the generalizability of the inferences you can make from geographical data by itself. So keep this in mind as we work through this chapter.

### 5. Types of geographical data available in Twitter

Location-based data can take on several different formats within the Twitter JSON object. Each of these has different levels of precision. On the most basic and imprecise level, someone can mention their location in the text of their tweet. Second, they can also mention their location in their user profile. Third, their location can be given by what's called a bounding box, which is a box of geographical coordinates drawn around their general area. Lastly, their exact location can be pinpointed by geographical coordinates and points.

### 6. Let's practice!

In the next lesson we're going to work with each of these types of data, but first we're going to make sure we know why we'd want to work with geographical data. 

## Geographical data in Twitter JSON

### 1. Geographical data in Twitter JSON

In this lesson, we'll learn about the different types of geographical data available in the Twitter JSON object.

### 2. Locations in Twitter text

A common -- although imprecise -- place where location data can be located is in the Twitter text itself. Users mention or allude to a location where they are or have visited. In the tweet above, I say that I'm in Zurich, indicating that they are at that particular location. These data may be helpful but are difficult to work with for several reasons. First, they require that some natural language processing detect that there is a location. Furthermore, and that the user is actually at that location. In the tweet above, I mention Zurich, but also Toronto. It's a non-trivial problem to decide that I am actually in Zurich, however. Lastly, once we have a location, we need to resolve it into a latitude and longitude.

### 3. User-defined location

Another value in the Twitter JSON which which offers location information is the user-defined location field. This is a text field which the user fills out. It can be found in the `location` field in the Twitter user JSON. While this may be a somewhat reliable place to look for location, there's a number of issues which arise with using this field. First, the location may be overly vague. In my profile above, you can see my location next to the Location Pin icon. I list my location as "Bay Area", which can cover quite a large geographic location. The location can also be some place vague, nonsensical, and fictional, such as "my bed" or "inside of Justin Bieber's heart". Lastly, it doesn't resolve to a specific longitude and latitude.

### 4. place JSON

The next value in which we can find geographical information is `place`. `place` is a child JSON object which contains several pieces of information. The most important of these is the `bounding_box`, which contains specific coordinates. The bounding box is a special kind of geographical object which allows Twitter to encode some uncertainty in location. As the box indicates, it is a set of four longitude and latitude coordinates which create a box which surrounds the location. This is the most common type of geographical object we'll find in Twitter data, so we'll spend the most time with this. The `place` object also includes the country, country code, full and short name of the place, and the place type, along with some identification data.

### 5. Calculating the centroid

The bounding box can range from a city block to a whole state or even country. For simplicity's sake, one way we can deal with handling these data is by translating the bounding box into what's called a centroid, or the center of the bounding box. You can see the centroid on the image being denoted by a small cross. The calculation of the centroid is straight-forward. We assume that there are only two unique longitudes and two unique latitudes. We obtain both of each of those values, then we add them up and divide them by two. This will find the midpoint of each side of the box.

### 6. coordinates JSON

The most specific type of geographical object is the `coordinates` object, which is, simply, a single set of longitude and latitude points. The `type` value indicates that this is a point, and the coordinates denote an exact place. However, given how exact this information is, even fewer tweets contain an exact place.

### 7. Let's practice!

In the following exercises, you're going to practice accessing these parts of the Twitter JSON and generate the centroid-calculating function. 

## Creating Twitter maps

### 1. Creating Twitter maps

Great job! Now that we've seen how to extract coordinates from Twitter data, we can start looking at how to create maps of tweets within Python. We'll learn how to render maps and how to plot points on those maps.

### 2. Introducing Basemap

Basemap is a library for plotting two dimensional maps in Python. Basemap is built on top of matplotlib, so much of the functionality that you got out of matplotlib applies maps you create with it. Basemap transforms coordinates into map projections, while matplotlib draws contours, lines, and points.

### 3. Beginning with Basemap

To create a new Basemap object, we'll first import it from the matplotlib toolkits. We then create a new Basemap object with five arguments. The first is the map projection we want to use. A map projection is the method for which we translate latitudes and longitudes of the Earth -- which is spherical -- onto a two-dimensional plane. Because we will be dealing with latitudes closer to the equator, we'll use the Mercator projection, which is denoted by 'merc'. We'll then provide coordinates for the lower level and upper right corners of the map. The second and third arguments, `llcrnrlat` and `llcrnrlon` stand for lower left corner latitude and lower left corner longitude, respectively. The same goes for the upper right latitudes and longitudes. Lastly, we'll make the continents white with the `fillcontinents` method, then draw the coast lines with gray with the `drawcoastlines` method, and draw the national borders with gray using the `drawcountries` method.

### 4. Plotting points

Now we add points to the map. To do this, we need a list of longitudes and latitudes. In this example, we're going to use the coordinates of capital cities for African countries. We'll first load the dataset. We'll then create the Basemap map. Next, we'll use the `fillcontinents` method, but we'll add a new argument, `zorder`, and set it zero. This ensures that the continents appear behind the points. Lastly, we'll use the `scatter` method and pass it the longitudes, then latitudes, to plot them. We need to let Basemap know that these values are latitudes and longitudes, so we'll set `latlon` equal to True. And we'll make them slightly transparent by setting alpha equal to 0.7.

### 5. Using color

Lastly, say we want to show some feature of a particular location, like whether Arabic is an official language. We can use a variable to denote that status and use a matplotlib to display it. In the `scatter` method, we add the argument `c` and pass it a list of that new variable. We need to also use `cmap` to select the matplotlib colormap we want to use. In the upcoming exercise, we're going to revisit a continuous variable -- Twitter sentiment -- to denote color.

### 6. Let's practice!

Now let's put some points on the map. We're going to plot tweets from the State of the Union dataset, as well as plot their sentiment. 