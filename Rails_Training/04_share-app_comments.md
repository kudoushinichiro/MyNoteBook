ネストについて
railsガイドから抜粋
https://railsguides.jp/routing.html#%E3%83%8D%E3%82%B9%E3%83%88%E3%81%97%E3%81%9F%E3%83%AA%E3%82%BD%E3%83%BC%E3%82%B9

>
2.7.2 「浅い」ネスト
前述したような深いネストを避けるひとつの方法として、コレクション (index/new/createのような、idを持たないアクション) だけを親のスコープの下で生成するという手法があります。このとき、メンバー (show/edit/update/destroyのような、idを必要とするアクション) をネストに含めないのがポイントです。これによりコレクションだけが階層化のメリットを受けられます。つまり、以下のように最小限の情報でリソースを一意に指定できるルーティングを作成するということです。

resources :articles do
  resources :comments, only: [:index, :new, :create]
end
resources :comments, only: [:show, :edit, :update, :destroy]
この方法は、ルーティングの記述を複雑にせず、かつ深いネストを作らないという絶妙なバランスを保っています。:shallowオプションを使うことで、上と同じ内容をさらに簡単に記述できます。

```ruby
resources :articles do
  resources :comments, shallow: true
end
```

