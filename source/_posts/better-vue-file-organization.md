title: Better Vue File Organization
date: 2017-08-09 20:55:28
tags:
  - vue
---

Before we see an alternative way to organize our vue files/components, I must say although I believe it's a better approach, in fact, there aren't _right_ or _wrong_ way, we should follow the way we feel more comfortable to work with.

The following piece of code was extracted from the [rss-reader](https://github.com/mrgodhani/rss-reader/blob/master/app/components/partials/sidebar.vue) project.

First, we'll see the original file, and after, the proposed way to organize it.

**The mental process**

1. Put **all** functions in the bottom of the file.
1. When a function needs to access `this` (the current context), we should pass it as the last parameter to our functions.
1. The `ctx` parameter you will see is an abbreviation to `context`.

**Reasons to change**

1. Better way to visualize all methods we have in the file.
1. Easier way to extract a function to another file if/when necessary.
1. More "functional" style (!?).

Let's see and compare the traditional approach with the suggested/alternative one.

**Traditional approach**

```js
<script>
import Feed from '../../helpers/feeds'
import Favicon from '../../helpers/favicon'
import queue from '../../helpers/queue'
import service from '../../helpers/services'
import { addArticles, addFeed } from '../../vuex/actions'
import _ from 'lodash'
import async from 'async'

export default {
  vuex: {
    getters: {
      offline: state => state.offline,
      feeds: state => state.feeds
    },
    actions: {
      addArticles,
      addFeed
    }
  },
  computed: {
    feedData () {
      return this.feeds.map(item => {
        if (item.title.length >= 20) {
          item.origtitle = item.title
        }
        item.title = _.truncate(item.title, { length: 20 })
        return item
      })
    }
  },
  data () {
    return {
      feedurl: '',
      alertmessage: '',
      showModal: false,
      processed: false
    }
  },
  methods: {
    allArticles () {
      return this.$route.router.go({path: '/', replace: true})
    },
    tags () {
      return this.$route.router.go({path: '/tags', replace: true})
    },
    favourites () {
      return this.$route.router.go({path: '/article/favourites'})
    },
    goFeed (title) {
      return this.$route.router.go({path: '/feed/' + title})
    },
    readArticles () {
      return this.$route.router.go({path: '/article/read'})
    },
    unreadArticles () {
      return this.$route.router.go({path: '/article/unread'})
    },
    fetchFeed (callback) {
      let feed = new Feed(this.feedurl)
      feed.init().then(result => {
        if (result === null) {
          let error = 'Sorry. I couldn\'t figure out any RSS feed on this address. Try to find link to RSS feed on that site by yourself and paste it here.'
          callback(error)
        } else {
          callback(null, result)
        }
      }, err => {
        if (err) {}
        let error = 'Sorry. Unfortunately this website is not supported.'
        callback(error)
      })
    },
    checkFeed (data, callback) {
      service.checkFeed(data.meta.title, count => {
        if (count === 0) {
          callback(null, data)
        } else {
          callback('Feed exists')
        }
      })
    },
    fetchIcon (data, callback) {
      let favicon = new Favicon(data.meta.link)
      favicon.init().then(result => {
        let path
        if (result !== null) {
          path = queue.queueTask('favicon', result)
        } else {
          path = null
        }
        data.meta.favicon = path
        data.meta.count = data.articles.length
        callback(null, data)
      })
    },
    addFeedItem (data, callback) {
      this.addFeed(data.meta, result => {
        data.meta = result
        callback(null, data)
      })
    },
    addArticleItems (data, callback) {
      data.articles.map(item => {
        let htmlFilename = queue.queueTask('html', item.link)
        item.feed = data.meta.title
        item.feed_id = data.meta._id
        item.file = htmlFilename
        item.favicon = data.meta.favicon
        return item
      })
      this.addArticles(data.articles)
      callback(null, 'done')
    },
    addFeedData () {
      let self = this
      this.processed = true
      async.waterfall([
        this.fetchFeed,
        this.checkFeed,
        this.fetchIcon,
        this.addFeedItem,
        this.addArticleItems
      ], (err, result) => {
        if (!err) {
          self.processed = false
          self.showModal = false
          this.feedurl = ''
        } else {
          self.alert = true
          self.alertmessage = err
          self.processed = false
        }
      })
    }
  }
}
</script>
```

**Alternative/suggested approach**

```js
<script>
import Feed from '../../helpers/feeds'
import Favicon from '../../helpers/favicon'
import queue from '../../helpers/queue'
import service from '../../helpers/services'
import { addArticles, addFeed } from '../../vuex/actions'
import _ from 'lodash'
import async from 'async'

export default {
  vuex: {
    getters: {
      offline: state => state.offline,
      feeds: state => state.feeds
    },

    actions: {
      addArticles,
      addFeed
    }
  },

  computed: {
    feedData: feedData(this)
  },

  data () {
    return {
      feedurl: '',
      alertmessage: '',
      showModal: false,
      processed: false
    }
  },

  methods: {
    allArticles() { allArticles(this) },
    tags() { tags(this) },
    favourites() { favourites(this) },
    goFeed() { goFeed(title, this) },
    readArticles() { readArticles(this) },
    unreadArticles() { unreadArticles(this) },
    fetchFeed() { fetchFeed(callback, this) },
    checkFeed() { checkFeed(data, callback) },
    fetchIcon() { fetchIcon (data, callback) },
    addFeedItem() { addFeedItem(data, callback, this) },
    addArticleItems() { addArticleItems(data, callback, this) },
    addFeedData() { addFeedData(this) }
  }
}

////////// Computed Properties
function feedData(ctx) {
  return ctx.feeds.map(item => {
    if (item.title.length >= 20) {
      item.origtitle = item.title
    }
    item.title = _.truncate(item.title, { length: 20 })
    return item
  })
}

////////// Methods
function allArticles(ctx) {
  return ctx.$route.router.go({path: '/', replace: true})
}

function tags(ctx) {
  return ctx.$route.router.go({path: '/tags', replace: true})
}

function favourites(ctx) {
  return ctx.$route.router.go({path: '/article/favourites'})
}

function goFeed(title, ctx) {
  return ctx.$route.router.go({path: '/feed/' + title})
}

function readArticles(ctx) {
  return ctx.$route.router.go({path: '/article/read'})
}

function unreadArticles(ctx) {
  return ctx.$route.router.go({path: '/article/unread'})
}

function fetchFeed(callback, ctx) {
  let feed = new Feed(ctx.feedurl)

  feed.init().then(result => {
    if (result === null) {
      let error = 'Sorry. I couldn\'t figure out any RSS feed on this address. Try to find link to RSS feed on that site by yourself and paste it here.'
      callback(error)
    } else {
      callback(null, result)
    }
  }, err => {
    if (err) {}
    let error = 'Sorry. Unfortunately this website is not supported.'
    callback(error)
  })
}

function checkFeed(data, callback) {
  service.checkFeed(data.meta.title, count => {
    if (count === 0) {
      callback(null, data)
    } else {
      callback('Feed exists')
    }
  })
}

function fetchIcon(data, callback) {
  let favicon = new Favicon(data.meta.link)
  favicon.init().then(result => {
    let path
    if (result !== null) {
      path = queue.queueTask('favicon', result)
    } else {
      path = null
    }
    data.meta.favicon = path
    data.meta.count = data.articles.length
    callback(null, data)
  })
}

function addFeedItem(data, callback, ctx) {
  ctx.addFeed(data.meta, result => {
    data.meta = result
    callback(null, data)
  })
}

function addArticleItems(data, callback, ctx) {
  data.articles.map(item => {
    let htmlFilename = queue.queueTask('html', item.link)
    item.feed = data.meta.title
    item.feed_id = data.meta._id
    item.file = htmlFilename
    item.favicon = data.meta.favicon
    return item
  })
  ctx.addArticles(data.articles)
  callback(null, 'done')
}

function addFeedData(ctx) {
  let self = ctx
  ctx.processed = true
  async.waterfall([
    ctx.fetchFeed,
    ctx.checkFeed,
    ctx.fetchIcon,
    ctx.addFeedItem,
    ctx.addArticleItems
  ], (err, result) => {
    if (!err) {
      self.processed = false
      self.showModal = false
      ctx.feedurl = ''
    } else {
      self.alert = true
      self.alertmessage = err
      self.processed = false
    }
  })
}
</script>
```

Which one do you prefer? Why? Share your thoughts with us :)
