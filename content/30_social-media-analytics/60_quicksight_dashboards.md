+++
title = "QuickSight Dashboards"
chapter = false
weight = 60
+++

### Create a Custom Query
1. Launch into QuickSight – https://us-east-1.quicksight.aws.amazon.com/sn/start.
2. Choose **Manage data** from the top right.
3. Choose **New Data Set**.
4. Create a new Athena Data Source.
5. Select the `socialanalyticsblog` database and the `tweet_sentiments` table.
6. Then choose **Edit/Preview Data**.
    ![quicksight table](/images/social-media-analytics/twitter-dashboard-17.gif)
7. Under **Table**, choose **Switch to custom SQL tool**:
    ![quicksight sql](/images/social-media-analytics/twitter-dashboard-18.gif)
8. Give the query a name (such as `SocialAnalyticsBlogQuery`)
9. Put in this query:

    ```sql
    SELECT  s.*,
            e.entity,
            e.type,
            e.score,
            t.lang as language,
            coordinates.coordinates[1] AS lon,
            coordinates.coordinates[2] AS lat ,
            place.name,
            place.country,
            t.timestamp_ms / 1000 AS timestamp_in_seconds,
            regexp_replace(source,
            '\<.+?\>', '') AS src
    FROM socialanalyticsblog.tweets t
    JOIN socialanalyticsblog.tweet_sentiments s
        ON (s.tweetid = t.id)
    JOIN socialanalyticsblog.tweet_entities e
        ON (e.tweetid = t.id)
    ```

10. Then choose Finish.
11. This saves the query and lets you see sampled data.
12. Switch the datatype for the `timestamp_in_seconds` to be a date:
    ![typechange](/images/social-media-analytics/twitter-dashboard-19.gif)
13. Then choose **Save and Visualize**.

### Create a Dashboard
Now we’ll step through creating a dashboard.

1. Start by making the first visual in the top-left quadrant of the display.
    ![quicksight](/images/social-media-analytics/twitter-dashboard-20.gif)
2. Drag and drop `type` and `tweetid` from the field list onto the visual.
3. Select the double arrow drop down next to Field Well.
    ![quicksight](/images/social-media-analytics/twitter-dashboard-21.gif)
4. Move the `tweetid` to the value.
5. And then choose it to perform Count Distinct:
    ![quicksight](/images/social-media-analytics/twitter-dashboard-22.gif)
6. Now switch it to a pie chart under visualization types.
    ![quicksight](/images/social-media-analytics/twitter-dashboard-23.gif)

    Now let’s add another visual.

7. Choose Add (near the top left corner of the page) : Add Visual.
8. Resize it and move it next to your first pie chart.
9. Now, on the left, drag over `sentiment` and `timestamp_in_seconds`.
10. Under the field wells, or the chart itself, you can zoom in/out of the time. Let’s zoom into hours:
    ![quicksight](/images/social-media-analytics/twitter-dashboard-25.gif)

11. Suppose on the timeline, we only want to see positive/negative/mixed sentiments. The Neutral line, at least for my Twitter terms, is causing the rest not to be seen easily.
    ![quicksight](/images/social-media-analytics/twitter-dashboard-26.gif)

12. Just click the Neutral line and in the box that appears choose to Exclude Neutral.
    ![quicksight](/images/social-media-analytics/twitter-dashboard-27.gif)
    ![quicksight](/images/social-media-analytics/twitter-dashboard-28.gif)

You can build multiple dashboards, zoom in and out of them, and see the data in different ways. Use the remaining time in this lab to try to build the dashboard below:
![quicksight](/images/social-media-analytics/twitter-dashboard-34.gif)