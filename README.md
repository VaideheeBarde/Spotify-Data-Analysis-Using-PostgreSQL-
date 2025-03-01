# Spotify-Data-Analysis-Using-PostgreSQL

**Overview** <br />
This project analyzes Spotify's database using PostgreSQL. It includes SQL queries that answer 13 analytical questions, categorized into Easy, Medium, and Advanced levels. The dataset was sourced from Kaggle and imported into PostgreSQL for querying.

**Technologies Used** <br />
PostgreSQL for database management and querying <br />
Kaggle as the data source <br />
SQL for data analysis <br />

**Dataset** <br />
The dataset contains details about tracks, albums, artists, and streaming statistics. <br />
This is where you can download the dataset - [Download the dataset here](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset) <br />

```
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```

**SQL Queries** <br />
**Easy Level** <br />
1. Retrieve the names of all tracks that have more than 1 billion streams.
```
SELECT * FROM spotify 
WHERE stream > 1000000000;
```
2. List all albums along with their respective artists.
```
SELECT
	DISTINCT album, artist
FROM spotify
ORDER BY 1;
```
3. Get the total number of comments for tracks where licensed = TRUE.
```
SELECT
	SUM(comments) as total_comments 
FROM spotify
WHERE licensed = 'true';
```
4. Find all tracks that belong to the album type single.
```
SELECT * FROM spotify 
WHERE album_type = 'single';
```
5. Count the total number of tracks by each artist.
```
SELECT 
	artist, --1
	COUNT(*) AS total_no_songs --2
FROM spotify
GROUP BY artist
ORDER BY 2
```

**Medium Level**
1. Calculate the average danceability of tracks in each album.
```
SELECT 
	album,
	avg(danceability) as avg_danceability
FROM spotify
GROUP BY 1
ORDER BY 2 DESC;
```
2. Find the top 5 tracks with the highest energy values.
```
SELECT 
	track, 
	MAX(energy)
FROM spotify
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```
3. List all tracks along with their views and likes where official_video = TRUE.
```
SELECT
	track,
	SUM(views) as total_views,
	SUM(likes) as total_likes
FROM spotify
WHERE official_video = 'true' 
GROUP BY 1
ORDER BY 2 DESC;
```
4. For each album, calculate the total views of all associated tracks.
```
SELECT 
	album,
	track,
	SUM(views) as total_views 
FROM spotify
GROUP BY 1, 2 
ORDER BY 3 DESC;
```
5. Retrieve the track names that have been streamed on Spotify more than YouTube.
```
SELECT * FROM 
(SELECT 
	track,
	COALESCE(SUM(CASE WHEN most_played_on='Youtube' THEN stream END), 0) as streamed_on_youtube,
	COALESCE(SUM(CASE WHEN most_played_on='Spotify' THEN stream END), 0) as streamed_on_spotify
FROM spotify
GROUP BY 1
) as t1
WHERE
	streamed_on_spotify > streamed_on_youtube
	AND
	streamed_on_youtube <> 0
```
**Advanced Level**
1. Find the top 3 most-viewed tracks for each artist using window functions.
2. Write a query to find tracks where the liveness score is above the average.
3. Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.

**How to Use** <br />
1. Clone this repository:
2. Import the dataset into PostgreSQL.
3. Run the SQL queries using a PostgreSQL client.

**Contribution** <br />
Feel free to fork this repository, suggest improvements, or add more queries!

