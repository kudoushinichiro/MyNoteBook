### rubocopの指摘修正について備忘録

rubocop実行時に以下の指摘２件



```
Offenses:

config/application.rb:29:34: C: Rails/FilePath: Please use Rails.root.join('path/to') instead.
    config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]
                                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
config/environments/development.rb:17:6: C: Rails/FilePath: Please use Rails.root.join('path/to') instead.
  if Rails.root.join('tmp', 'caching-dev.txt').exist?
     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
40 files inspected, 2 offenses detected


おそらくパスの表記の仕方を指摘されている。
該当ファイルについてそれぞれ以下の修正を施した

修正前(config/environments/development.rb)
```ruby
if Rails.root.join('tmp', 'caching-dev.txt').exist?
```
修正後(同ファイル)
```ruby
if Rails.root.join('tmp/caching-dev.txt').exist?
```


修正前(config/application.rb)
```ruby
config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]
```

修正後(同ファイル)
```ruby
config.i18n.load_path += Dir[Rails.root.join('config/locales/**/*.{rb,yml}').to_s]
```

再度rubocop実行
```
Inspecting 40 files
........................................

40 files inspected, no offenses detected
```

無事解消された。