# *Telegram*
<div class="alert alert-success" role="info">
  
  <span class="glyphicon glyphicon glyphicon-cog"></span>
  &nbsp; Telegram can be used as a part of [notifier](/Plugins/Notifiers) plugin system.
</div>
Send a message to one or more Telegram users or groups upon accepting a download.


## Preparations
<div class="alert alert-info" role="alert">
  <span class="glyphicon glyphicon glyphicon-download-alt"></span>
  &nbsp; Install `python-telegram-bot` python pkg
<br/><br/>

```bash
pip install python-telegram-bot
```
</div>

* Create a bot & obtain a token for it (see https://core.telegram.org/bots#botfather).
* For direct messages (not to a group), start a conversation with the bot and click `START` in the Telegram app.
* For group messages, add the bot to the desired group and send a start message to the bot: `/start` (mind the
  leading `/`).
## Configuration

| Option |Type|  Description | Default |
| --- | ---| --- |---|
|bot_token|text|Bot token. **Required**
|message|text| Notification message| Gets default from [notify](/Plugins/Notifiers/notify) plugin
|parse_mode|text|Message parsing. Either `html` or `markdown`. 
|recipients|text|List of recipients type. Can be `username`, `group` or `fullname`. See config example for details
| file_template | text|Name of the template file to use. See [notify](/Plugins/Notifiers/notify) plugin for more details| 
<div class="alert alert-info" role="info">
  
  <span class="glyphicon glyphicon-info-sign"></span>
  &nbsp; In case of message error when using `parse_mode`, the parsing will fall back to basic. This can be cause due to unclosed tags (watch out for wandering underscore when using markdown)
</div>
Not all Telegram users have a username. In such cases you would have to use the `fullname` approach. Otherwise, it is much easier to use the `username` configuration.

## Configuration example
```yaml
my-task:
  telegram:
    bot_token: token
    message: "{{title}}"
    parse_mode: markdown
    recipients:
      - username: my-user-name
      - group: my-group-name
      - fullname:
          first: my-first-name
          sur: my-sur-name
```

## Example using Jinja2 template and markdown
```yaml
telegram:
  telegram:
    bot_token: '{{secrets.credentials.telegram.bot_token}}'
    parse_mode: markdown
    recipients:
      - username: '{{secrets.credentials.telegram.username}}'
    message: |+
      {%if task in ["retreive_from_couchpotato","dynamic_imdb_actors"]%}*New movie added to queue*
      {%else%}*Download Started from task:
      *{{task|replace("_", "-")}}
      {%endif%}
      {% if series_name is defined -%}
      *{{series_name}}* - {{series_id}} - {{quality|d('')}}
      *{{tvmaze_episode_name|d(tvdb_ep_name)|d('')}}*
      [Image]({{tvmaze_series_original_image|replace("_", "%5F")}})
      [Show page]({{tvmaze_series_url|replace("_", "%5F")}})
      {% elif imdb_name is defined -%}
      *{{imdb_name}}* - ({{imdb_year}})
      {{quality|d('')}}
      {{imdb_score}}/10 - {{imdb_votes}} votes
      {{imdb_genres|join(', ')|title}} 
      *Plot:* {{imdb_plot_outline}}
      [Image]({{tmdb_posters[0]|replace("_", "%5F")}})
      [Movie Page]({{imdb_url|d('')}})
      {% else -%}
      {{title}}
      {%- endif -%}
```

