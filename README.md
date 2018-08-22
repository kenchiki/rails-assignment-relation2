# もう少し複雑なモデルの関連を作れるようになる

## 実行手順
- `bundle exec rake db:create`
- `bundle exec rake db:migrate`
- `bundle exec rails c`

```ruby
# Blogを登録
blog1 = Blog.create(title: 'ねこがすき！にゃんにゃんブログ')
blog2 = Blog.create(title: 'いぬがすき！わんわんブログ')
blog3 = Blog.create(title: 'つまがすき！いとうさんブログ')

# Entryを登録
entry1 = blog1.entries.create(title: 'はじめてのエントリー', body: 'はじめまして！')
entry2 = blog1.entries.create(title: '2番目のエントリー', body: 'おひさしぶりです！')
entry3 = blog3.entries.create(title: '3番目のエントリー', body: 'もうくじけました・・・')

# Commentを登録
entry1.comments.create(body: 'てすてす', status: 'approved')
entry1.comments.create(body: 'ねこはかわいいですよね', status: 'unapproved')
entry2.comments.create(body: '例のねこについて', status: 'approved')
entry3.comments.create(body: 'こんにちはこんにちは！', status: 'approved')

# Blogのid:1に紐づくすべてのCommentを表示
Comment.joins(entry: :blog).where(blogs: {id: 1})

# まだEntryを書いていないBlogを表示
Blog.includes(:entries).where(entries: {id: nil})

# statusがunapprovedであるCommentがあるEntryのあるBlogを表示
Blog.joins(entries: :comments).where(comments: {status: 'unapproved'})
```

## Ruby version
2.5.1
