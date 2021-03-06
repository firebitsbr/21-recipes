# Extracting Tweet Entities

## Problem

You want to extract tweet entities such as `@mentions`, `#hashtags`, and short URLs from Twitter search results or other batches of tweets.

## Solution

Use `rtweet::search_tweets()` or any of the _timeline_ functions in `rtweet`.

## Discussion

Michael has provided a very powerful search interface for Twitter data mining. `rtweet::search_tweets()` retrieves, parses and extracts an astounding amount of data for you to then use. Let's search Twitter for the `#rstats` hashtag and see what is available:


```r
library(rtweet)
library(tidyverse)
```

```r
(rstats <- search_tweets("#rstats", n=300)) # pull 300 tweets that used the "#rstats" hashtag
```

```
## # A tibble: 300 x 87
##    user_id  status_id  created_at          screen_name text         source
##    <chr>    <chr>      <dttm>              <chr>       <chr>        <chr> 
##  1 1328949… 994248165… 2018-05-09 16:10:09 RosanaFerr… RT @Rblogge… Hoots…
##  2 1074770… 994248170… 2018-05-09 16:10:10 eFe_Gil     "RT @dataan… Twitt…
##  3 32317906 994248406… 2018-05-09 16:11:07 agairola    "RT @SanaBe… Twitt…
##  4 14199849 994248592… 2018-05-09 16:11:51 simoncolum… RT @RLadies… Twitt…
##  5 15319524 994248693… 2018-05-09 16:12:15 straighted… Benefits fr… Twitt…
##  6 4174824… 994248940… 2018-05-09 16:13:14 Pvalsfr     RT @RLangTi… Twitt…
##  7 89191817 994248946… 2018-05-09 16:13:15 BigDataIns… "Big | Data… Paper…
##  8 2245869… 994249157… 2018-05-09 16:14:06 wytham88    "RT @marska… Twitt…
##  9 3163318… 994249403… 2018-05-09 16:15:04 GamerGeekN… "RT @tarask… RssTo…
## 10 3104058… 994249415… 2018-05-09 16:15:07 thermoio    "Web devs: … Hoots…
## # ... with 290 more rows, and 81 more variables: display_text_width <dbl>,
## #   reply_to_status_id <chr>, reply_to_user_id <chr>,
## #   reply_to_screen_name <chr>, is_quote <lgl>, is_retweet <lgl>,
## #   favorite_count <int>, retweet_count <int>, hashtags <list>,
## #   symbols <list>, urls_url <list>, urls_t.co <list>,
## #   urls_expanded_url <list>, media_url <list>, media_t.co <list>,
## #   media_expanded_url <list>, media_type <list>, ext_media_url <list>,
## #   ext_media_t.co <list>, ext_media_expanded_url <list>,
## #   ext_media_type <chr>, mentions_user_id <list>,
## #   mentions_screen_name <list>, lang <chr>, quoted_status_id <chr>,
## #   quoted_text <chr>, quoted_created_at <dttm>, quoted_source <chr>,
## #   quoted_favorite_count <int>, quoted_retweet_count <int>,
## #   quoted_user_id <chr>, quoted_screen_name <chr>, quoted_name <chr>,
## #   quoted_followers_count <int>, quoted_friends_count <int>,
## #   quoted_statuses_count <int>, quoted_location <chr>,
## #   quoted_description <chr>, quoted_verified <lgl>,
## #   retweet_status_id <chr>, retweet_text <chr>,
## #   retweet_created_at <dttm>, retweet_source <chr>,
## #   retweet_favorite_count <int>, retweet_retweet_count <int>,
## #   retweet_user_id <chr>, retweet_screen_name <chr>, retweet_name <chr>,
## #   retweet_followers_count <int>, retweet_friends_count <int>,
## #   retweet_statuses_count <int>, retweet_location <chr>,
## #   retweet_description <chr>, retweet_verified <lgl>, place_url <chr>,
## #   place_name <chr>, place_full_name <chr>, place_type <chr>,
## #   country <chr>, country_code <chr>, geo_coords <list>,
## #   coords_coords <list>, bbox_coords <list>, name <chr>, location <chr>,
## #   description <chr>, url <chr>, protected <lgl>, followers_count <int>,
## #   friends_count <int>, listed_count <int>, statuses_count <int>,
## #   favourites_count <int>, account_created_at <dttm>, verified <lgl>,
## #   profile_url <chr>, profile_expanded_url <chr>, account_lang <chr>,
## #   profile_banner_url <chr>, profile_background_url <chr>,
## #   profile_image_url <chr>
```

```r
glimpse(rstats)
```

```
## Observations: 300
## Variables: 87
## $ user_id                 <chr> "1328949324", "1074770804", "32317906"...
## $ status_id               <chr> "994248165174935554", "994248170241712...
## $ created_at              <dttm> 2018-05-09 16:10:09, 2018-05-09 16:10...
## $ screen_name             <chr> "RosanaFerrero", "eFe_Gil", "agairola"...
## $ text                    <chr> "RT @Rbloggers: An R Tutorial: Visual ...
## $ source                  <chr> "Hootsuite", "Twitter Web Client", "Tw...
## $ display_text_width      <dbl> 153, 140, 140, 140, 184, 140, 71, 140,...
## $ reply_to_status_id      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ reply_to_user_id        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ reply_to_screen_name    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ is_quote                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ is_retweet              <lgl> FALSE, TRUE, TRUE, TRUE, FALSE, TRUE, ...
## $ favorite_count          <int> 2, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 0, 0,...
## $ retweet_count           <int> 5, 24, 19, 11, 0, 4, 0, 12, 2, 0, 14, ...
## $ hashtags                <list> [<"rstats", "DataScience">, <"rstats"...
## $ symbols                 <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ urls_url                <list> ["wp.me/pMm6L-Gn7", "buff.ly/2wqNaEC"...
## $ urls_t.co               <list> ["https://t.co/Qyp2HpQapa", "https://...
## $ urls_expanded_url       <list> ["https://wp.me/pMm6L-Gn7", "https://...
## $ media_url               <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ media_t.co              <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ media_expanded_url      <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ media_type              <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ ext_media_url           <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ ext_media_t.co          <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ ext_media_expanded_url  <list> [NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ ext_media_type          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ mentions_user_id        <list> ["144592995", "3230388598", <"9619595...
## $ mentions_screen_name    <list> ["Rbloggers", "dataandme", <"SanaBenz...
## $ lang                    <chr> "en", "en", "en", "en", "en", "en", "e...
## $ quoted_status_id        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_text             <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_created_at       <dttm> NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ quoted_source           <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_favorite_count   <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_retweet_count    <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_user_id          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_screen_name      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_name             <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_followers_count  <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_friends_count    <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_statuses_count   <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_location         <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_description      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ quoted_verified         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ retweet_status_id       <chr> NA, "994200830088802304", "99417141657...
## $ retweet_text            <chr> NA, "\U0001f4c8 ggplot2 docs gettin' f...
## $ retweet_created_at      <dttm> NA, 2018-05-09 13:02:03, 2018-05-09 1...
## $ retweet_source          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ retweet_favorite_count  <int> NA, 115, 15, 9, NA, 8, NA, 38, 0, NA, ...
## $ retweet_retweet_count   <int> NA, 24, 19, 11, NA, 4, NA, 12, 2, NA, ...
## $ retweet_user_id         <chr> NA, "3230388598", "961959568808054784"...
## $ retweet_screen_name     <chr> NA, "dataandme", "SanaBenzi", "RLadies...
## $ retweet_name            <chr> NA, "Mara Averick", "SanaBenzi", "R- L...
## $ retweet_followers_count <int> NA, 24093, 125, 65, NA, 52016, NA, 380...
## $ retweet_friends_count   <int> NA, 2947, 283, 62, NA, 11, NA, 87, 216...
## $ retweet_statuses_count  <int> NA, 26660, 135, 66, NA, 1896, NA, 1534...
## $ retweet_location        <chr> NA, "Massachusetts", "Meudon, France",...
## $ retweet_description     <chr> NA, "tidyverse dev advocate @rstudio #...
## $ retweet_verified        <lgl> NA, FALSE, FALSE, FALSE, NA, FALSE, NA...
## $ place_url               <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ place_name              <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ place_full_name         <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ place_type              <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ country                 <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ country_code            <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA...
## $ geo_coords              <list> [<NA, NA>, <NA, NA>, <NA, NA>, <NA, N...
## $ coords_coords           <list> [<NA, NA>, <NA, NA>, <NA, NA>, <NA, N...
## $ bbox_coords             <list> [<NA, NA, NA, NA, NA, NA, NA, NA>, <N...
## $ name                    <chr> "Rosana Ferrero", "Fabian Gil", "Amit ...
## $ location                <chr> "", "", "Noida, India", "Amsterdam", "...
## $ description             <chr> "#DataScientist #ecologist Docente Mas...
## $ url                     <chr> "https://t.co/D2wcboyO2D", NA, NA, "ht...
## $ protected               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ followers_count         <int> 401, 130, 391, 956, 1738, 2130, 979, 1...
## $ friends_count           <int> 344, 110, 829, 379, 987, 1952, 1401, 4...
## $ listed_count            <int> 20, 130, 27, 63, 37, 7586, 218, 17, 20...
## $ statuses_count          <int> 1520, 4581, 4138, 9364, 4370, 460110, ...
## $ favourites_count        <int> 886, 1173, 14, 298, 8841, 1108, 29, 45...
## $ account_created_at      <dttm> 2013-04-05 11:15:34, 2013-01-09 20:12...
## $ verified                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ profile_url             <chr> "https://t.co/D2wcboyO2D", NA, NA, "ht...
## $ profile_expanded_url    <chr> "http://www.maximaformacion.es", NA, N...
## $ account_lang            <chr> "es", "es", "en", "en", "en", "fr", "e...
## $ profile_banner_url      <chr> "https://pbs.twimg.com/profile_banners...
## $ profile_background_url  <chr> "http://abs.twimg.com/images/themes/th...
## $ profile_image_url       <chr> "http://pbs.twimg.com/profile_images/4...
```

From the output, you can see that all the URLs (short and expanded), status id's, user id's and other hashtags are all available and all in a [tidy](http://r4ds.had.co.nz/tidy-data.html) data frame. 

What are the top 10 (with ties) other hashtags used in conjunction with `#rstats` (for this search group)?


```r
select(rstats, hashtags) %>% 
  unnest() %>% 
  mutate(hashtags = tolower(hashtags)) %>% 
  count(hashtags, sort=TRUE) %>% 
  filter(hashtags != "rstats") %>% 
  top_n(10)
```

```
## # A tibble: 12 x 2
##    hashtags            n
##    <chr>           <int>
##  1 datascience        51
##  2 python             45
##  3 bigdata            43
##  4 machinelearning    21
##  5 iot                20
##  6 opensource         18
##  7 c                  16
##  8 coding             16
##  9 java               16
## 10 php                16
## 11 programming        16
## 12 stem               16
```

## See Also

- Official Twitter [search API](https://developer.twitter.com/en/docs/tweets/search/guides/build-standard-query) documentation
- Twitter [entites](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/entities-object) information
- The [tidyverse](https://www.tidyverse.org/) introduction.
