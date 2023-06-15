+++
title = "The Dev.to Feed Algorithm 🤖"
date = 2020-01-23
description = """
Deep dive into dev.to code to find out how the feed works
"""
[extra]
+++

[TL;DR👇](#tl-dr)

## I used to develop apps. I still do, but I used to, too

Back in 2007/2008, I learned Ruby on Rails and developed two prototype sites that didn't end up in production. Since then, I did extensive work on non-Ruby, non-Rails server applications and learned enough about Android and iOS apps to manage the development of mobile apps in my current role.

I never touched Ruby on Rails again...until @anshbansal asked a question that I had asked myself a few times before.

> [What is the algorithm for dev.to's feed?](https://dev.to/anshbansal/what-is-the-algorithm-for-dev-to-s-feed-5h1c)

The following is my deep dive into the dev.to codebase to answer this question. There are probably a few things wrong, please point them out in the comments so I can correct them. Thank you.

## Start at the beginning

And it doesn't get much earlier than [the root route](https://github.com/thepracticaldev/dev.to/blob/33d6dca565dc5f544eaecac17612fca448357126/config/routes.rb#L409)
`root "stories#index"`

## Taking control

Rails follows a Model View Controller (MVC) architecture. When you ask dev.to to show you the root page, it will ask the stories controller to run the index action.

What we see there is it sets up a bunch of state then [renders the articles/index template](https://github.com/thepracticaldev/dev.to/blob/ba6c70c2d2198b84c3117e5d48e311f4cbbc1863/app/controllers/stories_controller.rb#L136)
`render template: "articles/index"`

## Show me the stories

If you inspect your dev.to home screen, you'll notice all the articles/stories are listed within an `articles-list` div. You can [find it in the articles/index view](https://github.com/thepracticaldev/dev.to/blob/7bfb255627a2664d76ed033da4c4ad03898b6bfb/app/views/articles/index.html.erb#L37) as expected. 

And here's where we start to see how the feed is populated.

## OK, first show me the featured story

The first story in the article list is a featured story.

The algorithm to get the featured story for a logged in user comes from [the stories controller](https://github.com/thepracticaldev/dev.to/blob/ba6c70c2d2198b84c3117e5d48e311f4cbbc1863/app/controllers/stories_controller.rb#L109) and [the articles/index view](https://github.com/thepracticaldev/dev.to/blob/7bfb255627a2664d76ed033da4c4ad03898b6bfb/app/views/articles/index.html.erb#L87). I've simplified it by substituting some variables and reorganizing some statements.

```ruby
@stories = Article.published.limited_column_select.page(1).per(35)
@stories = @stories.
  where("score > ? OR featured = ?", 9, true).
  order("hotness_score DESC")
offset = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
          1, 1, 1, 1, 2, 2, 2, 3, 3, 4, 
          5, 6, 7, 8, 9, 10, 11].sample # random offset, weighted more towards zero
@stories = @stories.offset(offset)

@featured_story = @stories.where.not(main_image: nil).first&.decorate || Article.new
```

In English:

> 1. Fetch a collection of stories that score above 9 or are featured
> 2. Order them, starting with the "hottest" one
> 3. Randomly skip the first 0 to 11 stories, weighted more towards 0
> 4. The featured story is the first story that has a main image

*Leaving how score, featured, and hotness are determined as an exercise for the reader*

Notice the featured article has nothing to do with which people, organizations, or tags you follow.

## Now show me the rest of the stories?

After rendering the featured story, the article/index view creates a `substories` div and then [renders the stories/main_stories_feed partial](https://github.com/thepracticaldev/dev.to/blob/7bfb255627a2664d76ed033da4c4ad03898b6bfb/app/views/articles/index.html.erb#L153)
`<%= render "stories/main_stories_feed" %>`

## These are not the divs you are looking for

I was scratching my head while reading through  [the _main_stories_feed partial](https://github.com/thepracticaldev/dev.to/blob/7bfb255627a2664d76ed033da4c4ad03898b6bfb/app/views/stories/_main_stories_feed.html.erb)

It populates the data attributes of a `new-articles-object` div and a `home-articles-object` div, then a bunch of other divs that have no contents. And the divs I do see when inspecting the home screen have the `single-article single-article-small-pic` class, but don't look like what's in this file.

Evil action-at-a-distance like this can only mean one thing: JavaScript

## Nobody expects the Spanish Inquisition

Searching the repo for `new-articles-object` and `home-articles-object`, we find them both in [initializeFetchFollowed Articles](https://github.com/thepracticaldev/dev.to/blob/c0a7c17054c027b461280ff9fd641a5e5594ca6a/app/assets/javascripts/initializers/initializeFetchFollowedArticles.js.erb#L34), called [very early](https://github.com/thepracticaldev/dev.to/blob/c0a7c17054c027b461280ff9fd641a5e5594ca6a/app/assets/javascripts/initializePage.js.erb#L4) when a page is initialized.

And there is a lot of logic here which I did not expect.

## The new stories are not the old stories

[The stories controller](https://github.com/thepracticaldev/dev.to/blob/ba6c70c2d2198b84c3117e5d48e311f4cbbc1863/app/controllers/stories_controller.rb#L113) populated the `@stories` collection used for the for the featured story. It is also used to populate the the data attributes of the `home-articles-object` div. But that comes next, not now. 

Instead, The first stories we see after the feature article are, populated from [a query directly in the view](https://github.com/thepracticaldev/dev.to/blob/7bfb255627a2664d76ed033da4c4ad03898b6bfb/app/views/stories/_main_stories_feed.html.erb#L3).

```ruby
@new_stories = Article.published.
  where("published_at > ? AND score > ?", rand(2..6).hours.ago, -15).
  limited_column_select.
  order("published_at DESC").
  limit(rand(15..80))
```

In English:

> 1. Fetch a collection of stories that have been published some time in the last 2 to 6 hours and score above -15
> 2. Order them by most recent first
> 3. Return the first 15 to 80 of them

Then the JavaScript function [`insertNewArticles`](https://github.com/thepracticaldev/dev.to/blob/c0a7c17054c027b461280ff9fd641a5e5594ca6a/app/assets/javascripts/initializers/initializeFetchFollowedArticles.js.erb#L33) takes over:

```javascript
articlesJSON.forEach(function(article){
      var articlePoints = 0
      var containsUserID = findOne([article.user_id], user.followed_user_ids || [])
      var containsOrganizationID = findOne([article.organization_id], user.followed_organization_ids || [])
      var intersectedTags = intersect_arrays(user.followed_tag_names, article.cached_tag_list_array)
      var followedPoints = 1
      var experienceDifference = Math.abs(article['experience_level_rating'] - user.experience_level || 5)
      var containsPreferredLanguage = findOne([article.language || 'en'], user.preferred_languages_array || ['en']);
      JSON.parse(user.followed_tags).map(function(tag) {
        if (intersectedTags.includes(tag.name)) {
          followedPoints = followedPoints + tag.points
        }
      })
      articlePoints = articlePoints + (followedPoints*2) + article.positive_reactions_count
      if (containsUserID || article.user_id === user.id) {
        articlePoints = articlePoints + 16
      }
      if (containsOrganizationID) {
        articlePoints = articlePoints + 16
      }
      if (containsPreferredLanguage) {
        articlePoints = articlePoints + 1
      } else {
        articlePoints = articlePoints - 10
      }
      var rand = Math.random();
      if (rand < 0.3) {
        articlePoints = articlePoints + 3
      } else if (rand < 0.6) {
        articlePoints = articlePoints + 6
      }
      articlePoints = articlePoints - (experienceDifference/2);
      article['points'] = articlePoints
    });
    var sortedArticles = articlesJSON.sort(function(a, b) {
      return b.points - a.points;
    });
    sortedArticles.forEach(function(article){
      var parent = insertPlace.parentNode;
      if ( article.points > 12 && !document.getElementById("article-link-"+article.id) ) {
        insertArticle(article,parent,insertPlace);
      }
    });
```

In English:

> 1. Give each article 0 points to start off with
> 2. Sum [the weight of each tag](https://dev.to/dashboard/following_tags) ([which can also be negative](https://github.com/thepracticaldev/dev.to/pull/1229)) the user follows and this article is tagged with, then double it
> 3. Now add to that, the number of positive reactions the article currently has
> 4. If the user follows the article's author, or is the articles author, add 16 points
> 5. If the user follows the article's organization, add 16 points
> 6. If the article is written in the user's language, add 1 point, otherwise, subtract 10 points
> 7. Randomly (with equal chance) give the article an extra 0, 3, or 6 points.
> 8. Subtract half the difference of this articles experience level vs [the user's experience](https://dev.to/settings/ux)
> 9. Order the articles by most points first
> 10. If the article has more than 12 points, show it to the user

## What about the rest?

The next batch of initialized articles come from [the same batch we got the featured article from](#ok-first-show-me-the-featured-story) and processed by a new (but familiar) algorithm in [`insertTopArticles`](https://github.com/thepracticaldev/dev.to/blob/c0a7c17054c027b461280ff9fd641a5e5594ca6a/app/assets/javascripts/initializers/initializeFetchFollowedArticles.js.erb#L84).

When you get to the bottom of that list, articles are populated from an [algoliasearch index of ordered articles](https://github.com/thepracticaldev/dev.to/blob/c0a7c17054c027b461280ff9fd641a5e5594ca6a/app/assets/javascripts/initializers/initializeFetchFollowedArticles.js.erb#L148). The [definition of that index](https://github.com/thepracticaldev/dev.to/blob/4e5dc015b377f922b96d698d4e8da2c46b8ff973/app/models/article.rb#L171) is found in the Article model.

Finally, scrolling kicks in which you can find in [initScrolling.js.erb](https://github.com/thepracticaldev/dev.to/blob/157a6f1ef17948cffcbe4997b85811cbc3be452d/app/assets/javascripts/initializers/initScrolling.js.erb#L203) and populates more articles from the algoliasearch index.

*Leaving the details of these as an exercise for the reader*

## TL;DR

For the first article in the list:

> 1. Fetch a collection of stories that score above 9 or are featured
> 2. Order them, starting with the "hottest" one
> 3. Randomly skip the first 0 to 11 stories, weighted more towards 0
> 4. The featured story is the first story that has a main image

For the next batch of articles:
> 1. Fetch a collection of stories that have been published some time in the last 2 to 6 hours and score above -15
> 2. Order them by most recent first
> 3. Return the first 15 to 80 of them
> 4. Give each article 0 points to start off with
> 5. Sum [the weight of each tag](https://dev.to/dashboard/following_tags) ([which can also be negative](https://github.com/thepracticaldev/dev.to/pull/1229)) the user follows and this article is tagged with, then double it
> 6. Now add to that, the number of positive reactions the article currently has
> 7. If the user follows the article's author, or is the articles author, add 16 points
> 8. If the user follows the article's organization, add 16 points
> 9. If the article is written in the user's language, add 1 point, otherwise, subtract 10 points
> 10. Randomly (with equal chance) give the article an extra 0, 3, or 6 points.
> 11. Subtract half the difference of this articles experience level vs [the user's experience](https://dev.to/settings/ux)
> 12. Order the articles by most points first
> 13. If the article has more than 12 points, show it to the user

If you've scrolled passed all of those,

> 1. Using the same collection the featured article came from
> 2. Process with [a similar but different algorithm as the previous batch](https://github.com/thepracticaldev/dev.to/blob/c0a7c17054c027b461280ff9fd641a5e5594ca6a/app/assets/javascripts/initializers/initializeFetchFollowedArticles.js.erb#L84)

And, finally

> [All articles ordered by hotness](https://github.com/thepracticaldev/dev.to/blob/4e5dc015b377f922b96d698d4e8da2c46b8ff973/app/models/article.rb#L197)

## Closing remarks

This could change at any time. For example, on 2019-09-19, @ben merged a PR to [add more variation to home feed](https://github.com/thepracticaldev/dev.to/pull/4069). All links to github are to the commit that I saw which was in `master` at the time of writing but, by the time you read this, `master` has probably moved on.