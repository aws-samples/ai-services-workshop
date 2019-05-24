+++
title = "Create Athena Tables"
chapter = false
weight = 50
+++

Now we can create our Amazon Athena tables. We have two options for this - one would be to have AWS Glue crawl the data and discover the schema - since we've already done this once we'll save the time of running a Glue crawler and instead manually create the tables and schemas.


First we'll go to the Athena console and run:
```sql
create database socialanalyticsblog
```

Now lets create a few tables.

{{% notice note %}}
Be sure to replace <TwitterRawLocation> with the corresponding output from our CloudFormation console.
{{% /notice %}}

```sql
CREATE EXTERNAL TABLE socialanalyticsblog.tweets (
	coordinates STRUCT<
		type: STRING,
		coordinates: ARRAY<
			DOUBLE
		>
	>,
	retweeted BOOLEAN,
	source STRING,
	entities STRUCT<
		hashtags: ARRAY<
			STRUCT<
				text: STRING,
				indices: ARRAY<
					BIGINT
				>
			>
		>,
		urls: ARRAY<
			STRUCT<
				url: STRING,
				expanded_url: STRING,
				display_url: STRING,
				indices: ARRAY<
					BIGINT
				>
			>
		>
	>,
	reply_count BIGINT,
	favorite_count BIGINT,
	geo STRUCT<
		type: STRING,
		coordinates: ARRAY<
			DOUBLE
		>
	>,
	id_str STRING,
	timestamp_ms BIGINT,
	truncated BOOLEAN,
	text STRING,
	retweet_count BIGINT,
	id BIGINT,
	possibly_sensitive BOOLEAN,
	filter_level STRING,
	created_at STRING,
	place STRUCT<
		id: STRING,
		url: STRING,
		place_type: STRING,
		name: STRING,
		full_name: STRING,
		country_code: STRING,
		country: STRING,
		bounding_box: STRUCT<
			type: STRING,
			coordinates: ARRAY<
				ARRAY<
					ARRAY<
						FLOAT
					>
				>
			>
		>
	>,
	favorited BOOLEAN,
	lang STRING,
	in_reply_to_screen_name STRING,
	is_quote_status BOOLEAN,
	in_reply_to_user_id_str STRING,
	user STRUCT<
		id: BIGINT,
		id_str: STRING,
		name: STRING,
		screen_name: STRING,
		location: STRING,
		url: STRING,
		description: STRING,
		translator_type: STRING,
		protected: BOOLEAN,
		verified: BOOLEAN,
		followers_count: BIGINT,
		friends_count: BIGINT,
		listed_count: BIGINT,
		favourites_count: BIGINT,
		statuses_count: BIGINT,
		created_at: STRING,
		utc_offset: BIGINT,
		time_zone: STRING,
		geo_enabled: BOOLEAN,
		lang: STRING,
		contributors_enabled: BOOLEAN,
		is_translator: BOOLEAN,
		profile_background_color: STRING,
		profile_background_image_url: STRING,
		profile_background_image_url_https: STRING,
		profile_background_tile: BOOLEAN,
		profile_link_color: STRING,
		profile_sidebar_border_color: STRING,
		profile_sidebar_fill_color: STRING,
		profile_text_color: STRING,
		profile_use_background_image: BOOLEAN,
		profile_image_url: STRING,
		profile_image_url_https: STRING,
		profile_banner_url: STRING,
		default_profile: BOOLEAN,
		default_profile_image: BOOLEAN
	>,
	quote_count BIGINT
) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
LOCATION '<TwitterRawLocation>';
```


Next we'll create tables for the tweet entities and tweet sentiments.

{{% notice note %}}
Remember to replace <TwitterEntitesLocation> and <TwitterSentimentLocation> with the corresponding outputs from CloudFormation.
{{% /notice %}}


```sql
CREATE EXTERNAL TABLE socialanalyticsblog.tweet_entities (
	tweetid BIGINT,
	entity STRING,
	type STRING,
	score DOUBLE
) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
LOCATION '<TwitterEntitiesLocation>';
```


```sql
CREATE EXTERNAL TABLE socialanalyticsblog.tweet_sentiments (
	tweetid BIGINT,
	text STRING,
	originalText STRING,
	sentiment STRING,
	sentimentPosScore DOUBLE,
	sentimentNegScore DOUBLE,
	sentimentNeuScore DOUBLE,
	sentimentMixedScore DOUBLE
) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
LOCATION '<TwitterSentimentLocation>'
```

Now we can write quereis against our tweet data! Let's see what comprehend discovered by grouping all of the tweets by the recognized entity types.
```sql
select type, count(*) cnt from socialanalyticsblog.tweet_entities
group by type order by cnt desc
```


