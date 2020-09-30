Others:
- design a graph database, backend using mySQL.
Answer using TAO, asked lots of API and schema questions

- ranking system index design (follow fb design)

- memocached and rocksdb related questions

- 具体讨论了了下P家每个pin背后都有⼀一个url，以及需要进⾏行行 web crawling来得到关于这个pin的信息，设计⼀一个这样的系统，有些什什么 tradeoff等等问题

- 设计⼀一个ACL serevice。 https://en.wikipedia.org/wiki/ Access_control_list 
- 设计个dashboard可以显示成千上万台机器器的qps， 90%latency 和 request success rate in nearly real-time. 如果要追求精确度 怎么办？ 答案是牺牲real time这个要求。。。 
- 和2有点类似的题⽬目， 在计算percentile的时候， 需要把每个 数据都发送到aggregator， 如果单机数据量量太⼤大没法发送怎么办？ 答案是 牺牲精确度使⽤用histogram。 然后⼀一堆followup。 --- 所有的设计其实都是 tradeoff， ⼀一切都是算计 

- 设计Twitter 问怎么search by  tags 感觉答的他不不是很满意 不不知道 是不不是要inverted  table之类的.

- notification system。 聊得很开放。也不不知道我答到点⼦子 没。我⾃自⼰己就说了了data model，还有cache，还有具体cache的数据结构 

- design inverted index

- distributed system

- 設計⼀一個餐廳推薦系統 (類類似Yelp)，假設餐廳有textual features (name, description), image和location。使⽤用者訊息有query, location, etc.1

- 設計⼀一個ad ranking系統。中間交雜著問了了⼀一些ML概念，像是ROC， precision/recall，model evaluation等等 

- design OOP 是 贪⻝⾷食蛇 pseudo code就可以 

- Design Pinterest


- to-read list
http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html (2013, may also outdated )

Two of my favorite lessons from the talk:
- Architecture is doing the right thing when growth can be handled by adding more of the same stuff. You want to be able to scale by throwing money at a problem which means throwing more boxes at a problem as you need them. If your architecture can do that, then you’re golden. 
- When you push something to the limit all technologies fail in their own special way. This lead them to evaluate tool choices with a preference for tools that are: mature; really good and simple; well known and liked; well supported; consistently good performers; failure free as possible; free. Using these criteria they selected: MySQL, Solr, Memcache, and Redis. Cassandra and Mongo were dropped.

Basics of Pinterest:
- Pins are an image with a collection of other bits of information, a description of what makes it important to the user, and link back to where they found it.
- Pinterest is a social network. You can follow people and boards.
- Database: They have users who have boards and boards have pins; follow and repin relationships; authentication information.

Tech stack:
- AWS EC2/S3 (reliability, good reporting and support, new instances available in seconds)
- MySQL (really mature, very solid, good community, totally free)
- Memcache
- Redis

Data:
- Pin, Board, User, URL
- Lookup table
- onjects and mapping 

http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html (2012, 50% user growth a month, fastest among history; use AWS, written in Python and Django; infra may be different nowadays;)