= Duplicates =

Take action on entries with duplicate field values

'''Syntax:'''

{{{
duplicates:
  field: <field name>
  action: [accept|reject]
}}}

'''Example:'''

Move duplicate movies for manual review

{{{
tasks:
  duplicate-movies:
    manual: yes
    seen: no
    filesystem: /storage/movies
    imdb_lookup: yes
    duplicates:
      field: imdb_id
      action: accept
    move:
      to: /storage/movies-duplicates
      allow_dir: yes
}}}