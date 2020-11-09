途中よくわからないところがあったので、デバックの備忘録

user.rbに<<idをしているケース

```
Processing by PostsController#index as HTML
  User Load (0.3ms)  SELECT  `users`.* FROM `users` WHERE `users`.`id` = 18 LIMIT 1
  ↳ app/controllers/posts_controller.rb:12
   (0.3ms)  SELECT `users`.`id` FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18
  ↳ app/models/user.rb:95
  Rendering posts/index.html.slim within layouts/application
  Post Load (0.3ms)  SELECT  `posts`.* FROM `posts` WHERE `posts`.`user_id` IN (1, 18) ORDER BY `posts`.`created_at` DESC LIMIT 15 OFFSET 0
  ↳ app/views/posts/index.html.slim:7
  User Load (0.2ms)  SELECT `users`.* FROM `users` WHERE `users`.`id` IN (18, 1)
  ↳ app/views/posts/index.html.slim:7
  Post Exists (0.2ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 11 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (2.0ms)
  Rendered posts/_like_area.html.slim (18.2ms)
  Post Exists (0.3ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 8 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.2ms)
  Rendered posts/_like_area.html.slim (1.6ms)
  Post Exists (0.2ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 5 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.1ms)
  Rendered posts/_like_area.html.slim (1.4ms)
  Post Exists (0.2ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 4 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.1ms)
  Rendered posts/_like_area.html.slim (1.3ms)
  Post Exists (0.2ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 2 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.1ms)
  Rendered posts/_like_area.html.slim (1.5ms)
  Rendered collection of posts/_post.html.slim [8 times] (35.1ms)
  User Load (0.6ms)  SELECT  `users`.* FROM `users` ORDER BY `users`.`created_at` DESC LIMIT 5
  ↳ app/views/posts/index.html.slim:23
  Rendered users/_follow_area.html.slim (2.1ms)
  User Exists (0.3ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 16 LIMIT 1
  ↳ app/models/user.rb:88
  Rendered users/_follow.html.slim (1.7ms)
  Rendered users/_follow_area.html.slim (3.9ms)
  User Exists (0.3ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 17 LIMIT 1
  ↳ app/models/user.rb:88
  Rendered users/_follow.html.slim (0.1ms)
  Rendered users/_follow_area.html.slim (2.2ms)
  User Exists (0.4ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 14 LIMIT 1
  ↳ app/models/user.rb:88
  Rendered users/_follow.html.slim (0.1ms)
  Rendered users/_follow_area.html.slim (2.4ms)
  User Exists (0.3ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 15 LIMIT 1
  ↳ app/models/user.rb:88
```

<<idをしていない

```
Processing by PostsController#index as HTML
  User Load (0.2ms)  SELECT  `users`.* FROM `users` WHERE `users`.`id` = 18 LIMIT 1
  ↳ app/controllers/posts_controller.rb:12
   (0.2ms)  SELECT `users`.`id` FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18
  ↳ app/models/user.rb:95
  Rendering posts/index.html.slim within layouts/application
  Post Load (0.3ms)  SELECT  `posts`.* FROM `posts` WHERE `posts`.`user_id` = 1 ORDER BY `posts`.`created_at` DESC LIMIT 15 OFFSET 0
  ↳ app/views/posts/index.html.slim:7
  User Load (0.1ms)  SELECT `users`.* FROM `users` WHERE `users`.`id` = 1
  ↳ app/views/posts/index.html.slim:7
  Post Exists (0.4ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 11 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (2.8ms)
  Rendered posts/_like_area.html.slim (7.1ms)
  Post Exists (0.3ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 8 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.2ms)
  Rendered posts/_like_area.html.slim (1.7ms)
  Post Exists (0.3ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 5 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.2ms)
  Rendered posts/_like_area.html.slim (1.6ms)
  Post Exists (0.2ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 4 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.1ms)
  Rendered posts/_like_area.html.slim (1.4ms)
  Post Exists (0.2ms)  SELECT  1 AS one FROM `posts` INNER JOIN `likes` ON `posts`.`id` = `likes`.`post_id` WHERE `likes`.`user_id` = 18 AND `posts`.`id` = 2 LIMIT 1
  ↳ app/models/user.rb:71
  Rendered posts/_like.html.slim (0.1ms)
  Rendered posts/_like_area.html.slim (1.2ms)
  Rendered collection of posts/_post.html.slim [5 times] (22.1ms)
  User Load (0.3ms)  SELECT  `users`.* FROM `users` ORDER BY `users`.`created_at` DESC LIMIT 5
  ↳ app/views/posts/index.html.slim:23
  Rendered users/_follow_area.html.slim (1.7ms)
  User Exists (0.3ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 16 LIMIT 1
  ↳ app/models/user.rb:88
  Rendered users/_follow.html.slim (1.7ms)
  Rendered users/_follow_area.html.slim (3.1ms)
  User Exists (0.3ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 17 LIMIT 1
  ↳ app/models/user.rb:88
  Rendered users/_follow.html.slim (0.1ms)
  Rendered users/_follow_area.html.slim (1.4ms)
  User Exists (0.2ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 14 LIMIT 1
  ↳ app/models/user.rb:88
  Rendered users/_follow.html.slim (0.1ms)
  Rendered users/_follow_area.html.slim (1.2ms)
  User Exists (0.2ms)  SELECT  1 AS one FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18 AND `users`.`id` = 15 LIMIT 1
  ↳ app/models/user.rb:88
```

18が自分のid
<<idした時

```
SELECT `users`.`id` FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18
  ↳ app/models/user.rb:95
SELECT  `posts`.* FROM `posts` WHERE `posts`.`user_id` IN (1, 18) ORDER BY `posts`.`created_at` DESC LIMIT 15 OFFSET 0
  ↳ app/views/posts/index.html.slim:7
```

```
<<外した時（エラー）
SELECT `users`.`id` FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18
  ↳ app/models/user.rb:95
SELECT  `posts`.* FROM `posts` WHERE `posts`.`user_id` = 1 ORDER BY `posts`.`created_at` DESC LIMIT 15 OFFSET 0
  ↳ app/views/posts/index.html.slim:7
```

<<17とした時

```
SELECT `users`.`id` FROM `users` INNER JOIN `relationships` ON `users`.`id` = `relationships`.`followed_id` WHERE `relationships`.`follower_id` = 18
  ↳ app/models/user.rb:95
SELECT  `posts`.* FROM `posts` WHERE `posts`.`user_id` IN (1, 17) ORDER BY `posts`.`created_at` DESC LIMIT 15 OFFSET 0
  ↳ app/views/posts/index.html.slim:7
```
