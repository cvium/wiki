# Movie List
<div class="alert alert-success" role="info">
  
  <span class="glyphicon glyphicon glyphicon-cog"></span>
  &nbsp; This is part of [managed list](/Plugins/List) plugin system.
</div>

<div class="alert alert-info" role="info">
  
  <span class="glyphicon glyphicon glyphicon-info-sign"></span>
  &nbsp; To improve matching, make sure you have a lookup plugin enabled, eg. [imdb_lookup](/Plugins/imdb_lookup).
</div>

Any entry containing field(s) `imdb_id`, `trakt_movie_id` or `tmdb_id` can be added to movie list for later matching. This allows user to maintain one or more movie queues.

## Schema

```text
movie_list: <name>
```

Or:

```text
movie_list: 
  list_name: <name>
  [strip_year]: <yes|no>
```

**Clarification**: By default, entries that are generated from `movie_list` include the movie year (if available) in the title. Using `strip_year` only affects how `movie_list` **OUTPUTS** the title and not how it stores it. In other words, this option is only relevant when using `movie_list` as an input, either by itself in a task or when using [discover](/Plugins/discover) plugin.

### Matching
When using `movie_list` with a filter like [list_match](/Plugins/List/list_match) for example, the identifiers take precedence in matching over title, meaning that if the movie has an identifier and an entry with that same identifier type and value is examined, that movie will be matched. Thus it's preferable to use a lookup plugin in the task, preferably, the same one(s) that was used to add movie to the list.
If matching by identifiers fail, then title is [parsed](/Plugins/parsing) and proceeded to try to match by `movie_name` and `movie_year` fields.

## Usage
As a [managed list](/Plugins/List) plugin it follows the same list actions, an quick example how to add entries into the list.

```yaml
# inputs
# filters
list_add: 
  - movie_list: list name
```

### Fill list
```yaml
trakt_list:
  username: traktusername
  list: watchlist
  type: movies 
accept_all: yes
list_add:
  - movie_list: movies from trakt
```

This will add all accepted entries to an `movie_list` with the name `movies from trakt`. It then later be used as an input itself. This can be used a base to filter on with other tasks. You can add multiple inputs if you wish.

```yaml
trakt_list:
  username: traktusername
  list: watchlist
  type: movies 
imdb_list:
  login: email@123.com
  password: flexget
  list: watchlist
list_add:
  - movie_list: all my watchlists
```

### Download matches
```yaml
rss: ...
list_match:
  from:
    - movie_list: movie from trakt
download: ...
```

Concrete example with a task:

```yaml
task_name:
  rss: http://url.com/feed.xml
  quality: 720p
  imdb_lookup: yes
  list_match:
    from:
      - movie_list: movie list name
  download: /path/to/download
```

[quality](/Plugins/quality) is used to weed out any unwanted qualities. [imdb_lookup](/Plugins/imdb_lookup) is required for entries to be matched with movie_list.

### With discover
```yaml
discover-movies:
  discover:
    what:
      - movie_list: movie list name
    from:
      - rarbg: <opts>
  quality: 720p
  imdb_lookup: yes
  list_match:
    from:
      - movie_list: movie list name
  download: /path/to/download
```

**Strip Year option**: It is sometimes required by some search plugin to remove the year for the movie title. If that's the case, the following config can be used:

```yaml
discover-movies:
  discover:
    what:
      - movie_list: 
          list_name: movie list name
          strip_year: yes
    from:
      - rarbg: <opts>
  quality: 720p
  imdb_lookup: yes
  list_match:
    from:
      - movie_list: movie list name
  download: /path/to/download
```

The [list_match](/Plugins/List/list_match) matches only the first item from the list by default. See its wiki for different options.

Plugin [seen](/Plugins/seen) needs to be disabled since all the titles that movie queue will emit were seen when they were added to the queue

## Movie list CLI

See [movie-list commandline](/CLI/movie-list) how to view and manage the list via commandline.


## Movie List API
`movie_list` plugin has full API support. See [API post](http://discuss.flexget.com/t/flexget-rest-api/) for details