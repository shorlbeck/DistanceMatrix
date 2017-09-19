# DistanceMatrix PHP API Wrapper

Very simple API Wrapper for Google's DistanceMatrix API. Enter two addresses and the class returns the distance between them in meter. Alternatively use Google Static Map API to generate a map with a line plotted between two adresses.

## Install

Include DistanceMatrix.php in your project.


## Basic usage
Calculate the distance in meters between the [Inktweb.nl office](https://www.inktweb.nl/) and Paleis Noordeinde in the Hague.
```php
use MartijnOud\DistanceMatrix\DistanceMatrix;

$distanceMatrix = new DistanceMatrix(YOUR_API_KEY_HERE);

$result = $distanceMatrix->distance([
    'origins' => 'Prof. van der Waalsstraat 2 Alkmaar',
    'destinations' => 'Paleis Noordeinde Den Haag'
]);

if ($result["distance"] > 0) {
	$distance = round($result["distance"] / 1000, 2) . "km"; // 84.5km
}

//you can also get the travel estimate time
$duration = round($result["duration"] / 60) . " minutes"; // Seconds to minutes

if ($duration >= 60) { // If $duration is greater of equal 60 minutes, we devide again by 60 and get hours
	$duration = round($duration / 60, 1) . " hours";
}

echo "Distance:" . $distance . " - Duration: " . $duration;

```

## More control
```php
use MartijnOud\DistanceMatrix\DistanceMatrix;

$distanceMatrix = new DistanceMatrix(YOUR_API_KEY_HERE);

$result = $distanceMatrix->distance([
	'origins' => 'Leith',
	'destinations' => 'Arques',
	'mode' => 'walking',
	'language' => 'en-GB',
]);

if ($result["distance"] > 0) {
	echo "I would walk " . $result["distance"] * 0.00062137119 . " miles"; // I would walk 493.88322020532 miles
}
````

## Generating a map with directions
Make sure to enable API capabilities in your developer console:  Google Maps Directions, Google Maps Distance Matrix API
```php
use MartijnOud\DistanceMatrix\DistanceMatrix;

$distanceMatrix = new DistanceMatrix(YOUR_API_KEY_HERE);

$image = $distanceMatrix->mapDirections(array(
	'origins' => 'Prof. van der Waalsstraat 2 Alkmaar', // required
	'destinations' => 'Amsterdam', // required
	'size' => '728x200',
	'scale' => 1)
);

if ($image) {
	echo '<img src="'.$image.'" />';
}

```

## Generating a map
An API key is not required for this. If you do supply a key make sure this key has premission to use the Static Map API.
```php
use MartijnOud\DistanceMatrix\DistanceMatrix;

$distanceMatrix = new DistanceMatrix();

$image = $distanceMatrix->map([
	'origins' => 'Prof. van der Waalsstraat 2 Alkmaar', // required
	'destinations' => 'Amsterdam', // required
	'size' => '728x200'
]);

if ($image) {
	echo '<img src="'.$image.'" />';
}
```
![google-static-map](https://cloud.githubusercontent.com/assets/1292436/8251065/aba708ce-1679-11e5-8f26-95f8627fcb63.png)



## Todo

Ideas for future versions.

- [ ] Better error handling, checking rate limit, etc
- [x] Show a map with a line plotted between two points
