## What are common database development mistakes made by application developers?
## Những lỗi phổ biến nào mà các nhà phát triển ứng dụng tạo ra khi thiết kế database ?

_nguồn https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Not using appropriate indices**
**1. Không sử dụng các chỉ số thích hợp**

Đây là điều khá đơn giản nhưng nó vẫn xảy ra mọi lúc. Các khóa ngoại nên có các indexs. Nếu bạn đang sử dụng một trường trong một lệnh WHERE thì bạn nên (hoặc có thể) có một index trong câu lệnh này. Các chỉ mục như vậy thường bao gồm nhiều cột dựa trên các truy vấn bạn cần thực hiện

**2. Không thực thi toàn vẹn tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi, nhưng nếu cơ sở dữ liệu của bạn hỗ trợ tham chiếu toàn vẹn- có nghĩa là tất cả các khóa ngoại phải được đảm bảo luôn tham chiếu đến một thưc thể nào đó đã tồn tại--bạn nên dùng nó.

Điều này được thấy khá phổ biến trong cơ sở dữ liệu MySql. Tôi không tin là MyISAM hỗ trợ nó. Bạn sẽ không thể biết được ai đang sử dụng MyISAM hay InnoDB.

 Xem thêm tại đây:

- [How important are constraints like NOT NULL and FOREIGN KEY if I’ll always control my database input with php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Are foreign keys really necessary in a database design?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Are foreign keys really necessary in a database design?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng các khóa chính thay mặt thay vì các khóa chính tự nhiên **

Các natural key là những khóa trên cơ sở các dữ liệu duy nhất có nghĩa bên ngoài. Các ví dụ điển hình là các mã sản phẩm, 2 chữ cái mã tiểu bang (US), mã số bảo mật mạng xã hội, vân vân,...Các khóa chính kỹ thuật và các khóa chính tự nhiên hoàn toàn không có ý nghĩa bên ngoài hệ thống của ta. Chúng được tạo ra hoàn toàn để xác định thực thể và thông thường các trường (trong SQL Server, MySQL, ...) hoặc chuỗi (trong Oracle) tự động tăng giá trị  hoặc các trình tự (nhất là Oracle).

Theo ý kiến của tôi thì bạn nên **luôn luôn ** sử dụng các khóa kỹ thuật, vấn đề này được đề ra trong những câu hỏi dưới đây:

- [How do you like your primary keys?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [What's the best practice for primary keys in tables?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Which format of primary key would you use in this situation.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Surrogate vs. natural/business keys](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Should I have a dedicated primary key field?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là một chủ đề gây nhiều tranh cãi mà sẽ không nhận được sự đồng tình của tất cả mọi người. Trong khi bạn có thể thấy có người nghĩ là sử dụng các khóa tự nhiên trong một vài trường hợp là OK nhưng bạn sẽ không tìm thấy bất kỳ ý kiến nào cho rằng các khóa tự nhiên là không cần thiết. Nếu bạn hỏi tôi thì đó là một nhược điểm khá nhỏ.

Hãy nhớ rằng các quốc gia cũng có thể không tồn tại (ví dụ Yugoslavia).

**4. Viết các truy vấn yêu cầu

DISTINCT để thực hiện công việc**

Bạn thường gặp nó trong các truy vấn được tạo bởi ORM. Hãy xem phần log của Hibernate và bạn sẽ thấy tất cả các truy vấn đều bắt đầu với

SELECT DISTINCT ..

Điều này ngắn gọn là để đảm bảo bạn không lấy ra các bản ghi giống nhau dẫn tới việc lấy ra các đối tượng bị sao chép. Đôi khi bạn sẽ thấy mọi người đang làm điều này tất tốt. Nếu bạn xem nó quá nhiều thì nó là một lá cờ đỏ thực sự. Điều đó không có nghĩa là DISTINCT là không tốt hay không có các ứng dụng hợp lệ. Nó đều bao gồm cả 2 mặt nhứng không phải là câu lệnh 

DISTINCT is bad or doesn't have valid applications. It does (on both counts) but it's not a surrogate or a stopgap for writing correct queries.

Lấy từ [Why I Hate DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):
> Lúc mà có nhiều thứ trở nên không đúng là lúc mà 1 nhà phát triển đang xây dựng các truy vấn lồng nhau, join các bảng với nhau, và đột nhiên ông nhận ra rằng có vẻ như ông đang nhận được các bản ghi lặp lại (hoặc thậm chí nhiều hơn) và phản ứng ngay lập tức ... "giải pháp" của ông ta đối với "vấn đề" này là ném vào truy vấn từ khóa DISTINCT và POOF,  tất cả các rắc rối của ông biến mất.

**5. Sử dụng các lệnh kết hợp lại thay vì lệnh join**

Một sai lầm phổ biến khác của các nhà phát triển ứng dụng cơ sở dữ liệu là không nhận ra các lệnh kết hợp với nhau sẽ tiêu tốn hơn so với các lệnh join (ví dụ Mệnh đề GROUP BY) khi đem so sánh với nhau.

Để cho bạn thấy được vấn đề này phổ biến như nào, tôi đã từng đề cập vấn đề này một vài lần trong các bài viết dưới đây và được downvoted rất nhiều. Ví dụ:

To give you an idea of how widespread this is, I've written on this topic several times here and been downvoted a lot for it. For example:

Lấy từ [SQL statement - “join” vs “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> Truy vấn thứ nhất:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Thời gian thực hiện: 0.312 s

> Truy vấn thứ 2:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Thời gian thực hiện: 0.016 s

> Đã rõ. Trong câu lệnh join tôi đã chỉ ra là nó **nhanh hơn 20 lần so với câu câu lệnh sử dụng các lệnh kết hợp.**

**6. Không đơn giản hóa các truy vấn phức tạp thông qua views**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ views nhưng đối với những người làm, họ có thể rất đơn giản hóa các truy vấn nếu được sử dụng một cách khéo léo. Ví dụ, trong một dự án tôi đã sử dụng một mô hình [generic Party](http://www.tdan.com/view-articles/5014/) cho CRM. Đây là một kỹ thuật mô hình rất mạnh và linh hoạt nhưng có thể cần sử dụng đến nhiều lệnh join. Trong mô hình này gồm có:

- **Party**: con người và các tổ chức;
- **Party Role**: những điều mà các bên đã làm, ví dụ như Employee và Employer;
- **Party Role Relationship**: những chức vụ này có liên quan với nhau như thế nào.

Ví dụ:

- Ted là một Person, và là kiểu dữ liệu con của một Party;
- Ted có nhiều roles, trong đó có Employee;
- Intel là một organisation, là kiểu dữ liệu con của một Party;
- Intel có nhiều roles, trong đó có Employer;
- Intel thuê Ted, nghĩa là có một quan hệ giữa các role của 2 bên.

Vì vậy sẽ có 5 bảng để được join lại để liên kết Ted với employer tương ứng. Bạn giả sử tất cả các employees là People (không phải là organasation)và đưa ra view trợ giúp sau:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và đột nhiên bạn có một cái nhìn rất đơn giản về dữ liệu mà bạn muốn nhưng trên một mô hình dữ liệu có tính linh hoạt cao.

**7. Không lọc đầu vào**

Đây là một lỗi lớn. Bây giờ tôi thích PHP nhưng nếu bạn không biết bạn đang làm gì thì thật sự dễ để tạo các trang web dễ bị tấn công. Việc tổng hợp lỗi này được thông kế đầy đủ trong [story of little Bobby Tables](http://xkcd.com/327/).

Dữ liệu cấp bởi người dùng thông qua URL, form và **cookie** nên sẽ luôn luôn bị xem là dễ bị tấn công và lọc nhất. 

Data provided by the user by way of URLs, form data **and cookies** should always be treated as hostile and sanitized. Đảm bảo bạn đang nhận được những gì bạn muốn.

**8. Không sử dụng các câu lệnh được lấy sẵn trước đó**

Các câu lệnh đã được lấy sẵn trước đó là khi bạn biên dịch 1 câu lệnh, trừ các lệnh insert, update và các mệnh để where và các thành phần theo sau. Ví dụ 

SELECT * FROM users WHERE username = 'bob'

với

SELECT * FROM users WHERE username = ?

hoặc

SELECT * FROM users WHERE username = :username

phụ thuộc vào nền tảng của bạn.

Tôi đã nhìn thấy cơ sở dữ liệu mang đến đầu gối của họ bằng cách làm điều này . Về cơ bản, mỗi khi cơ sở dữ liệu hiện đại gặp một truy vấn mới, nó phải biên dịch nó. Nếu nó gặp một truy vấn nó được nhìn thấy trước, bạn đang tạo cho cơ sở dữ liệu có thể cache truy vấn đã biên dịch và dự định có thể thực thi. Bằng cách thực hiện các truy vấn `nhieeuf` nhiều bạn đang giúp cơ sở dữ liệu có thể tính toán và tối ưu hóa cho phù hợp (ví dụ, bằng cách ghim các truy vấn đã được biên dịch trong bộ nhớ) 

Sử dụng các câu lệnh được lấy sẵn cũng sẽ cung cấp cho bạn số liệu thống kê có ý nghĩa về tần suất các truy vấn được sử dụng.

Các lệnh được lấy sẵn cũng sẽ bảo vệ khỏi các tấn công SQL injection tốt hơn

**9. Not normalizing enough**

[Database normalization](http://en.wikipedia.org/wiki/Database_normalization) is basically the process of optimizing database design or how you organize your data into tables.

Just this week I ran across some code where someone had imploded an array and inserted it into a single field in a database. Normalizing that would be to treat element of that array as a separate row in a child table (ie a one-to-many relationship).

This also came up in [Best method for storing a list of user IDs](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> I've seen in other systems that the list is stored in a serialized PHP array.

But lack of normalization comes in many forms.

More:

- [Normalization: How far is far enough?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Why You Need Database Normalization ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Normalizing too much**

This may seem like a contradiction to the previous point but normalization, like many things, is a tool. It is a means to an end and not an end in and of itself. I think many developers forget this and start treating a "means" as an "end". Unit testing is a prime example of this.

I once worked on a system that had a huge hierarchy for clients that went something like:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

such that you had to join about 11 tables together before you could get any meaningful data. It was a good example of normalization taken too far.

More to the point, careful and considered denormalization can have huge performance benefits but you have to be really careful when doing this.

More:

- [Why too much Database Normalization can be a Bad Thing](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [How far to take normalization in database design?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [When Not to Normalize your SQL Database](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Maybe Normalizing Isn't Normal](http://www.codinghorror.com/blog/archives/001152.html)
- [The Mother of All Database Normalization Debates on Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Using exclusive arcs**

An exclusive arc is a common mistake where a table is created with two or more foreign keys where one and only one of them can be non-null.  **Big mistake.** For one thing it becomes that much harder to maintain data integrity. After all, even with referential integrity, nothing is preventing two or more of these foreign keys from being set (complex check constraints notwithstanding).

From [A Practical Guide to Relational Database Design](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> We have strongly advised against exclusive arc construction wherever possible, for the good reason that they can be awkward to write code and pose more maintenance difficulties.

**12. Not doing performance analysis on queries at all**

Pragmatism reigns supreme, particularly in the database world. If you're sticking to principles to the point that they've become a dogma then you've quite probably made mistakes. Take the example of the aggregate queries from above. The aggregate version might look "nice" but its performance is woeful. A performance comparison should've ended the debate (but it didn't) but more to the point: spouting such ill-informed views in the first place is ignorant, even dangerous.

**13. Over-reliance on UNION ALL and particularly UNION constructs**

A UNION in SQL terms merely concatenates congruent data sets, meaning they have the same type and number of columns. The difference between them is that UNION ALL is a simple concatenation and should be preferred wherever possible whereas a UNION will implicitly do a DISTINCT to remove duplicate tuples.

UNIONs, like DISTINCT, have their place. There are valid applications. But if you find yourself doing a lot of them, particularly in subqueries, then you're probably doing something wrong. That might be a case of poor query construction or a poorly designed data model forcing you to do such things.

UNIONs, particularly when used in joins or dependent subqueries, can cripple a database. Try to avoid them whenever possible.

**14. Using OR conditions in queries**

This might seem harmless. After all, ANDs are OK. OR should be OK too right? Wrong. Basically an AND condition **restricts** the data set whereas an OR condition **grows** it but not in a way that lends itself to optimisation. Particularly when the different OR conditions might intersect thus forcing the optimizer to effectively to a DISTINCT operation on the result.

Bad:

... WHERE a = 2 OR a = 5 OR a = 11

Better:

... WHERE a IN (2, 5, 11)

Now your SQL optimizer may effectively turn the first query into the second. But it might not. Just don't do it.

**15. Not designing their data model to lend itself to high-performing solutions**

This is a hard point to quantify. It is typically observed by its effect. If you find yourself writing gnarly queries for relatively simple tasks or that queries for finding out relatively straightforward information are not efficient, then you probably have a poor data model.

In some ways this point summarizes all the earlier ones but it's more of a cautionary tale that doing things like query optimisation is often done first when it should be done second. First and foremost you should ensure you have a good data model before trying to optimize the performance. As Knuth said:

> Premature optimization is the root of all evil

**16. Incorrect use of Database Transactions**

All data changes for a specific process should be atomic. I.e. If the operation succeeds, it does so fully. If it fails, the data is left unchanged. - There should be no possibility of 'half-done' changes.

Ideally, the simplest way to achieve this is that the entire system design should strive to support all data changes through single INSERT/UPDATE/DELETE statements. In this case, no special transaction handling is needed, as your database engine should do so automatically.

However, if any processes do require multiple statements be performed as a unit to keep the data in a consistent state, then appropriate Transaction Control is necessary.

- Begin a Transaction before the first statement.
- Commit the Transaction after the last statement.
- On any error, Rollback the Transaction. And very NB! Don't forget to skip/abort all statements that follow after the error.

Also recommended to pay careful attention to the subtelties of how your database connectivity layer, and database engine interact in this regard. 

**17. Not understanding the 'set-based' paradigm**

The SQL language follows a specific paradigm suited to specific kinds of problems. Various vendor-specific extensions notwithstanding, the language struggles to deal with problems that are trivial in langues like Java, C#, Delphi etc.

This lack of understanding manifests itself in a few ways.

- Inappropriately imposing too much procedural or imperative logic on the databse.
- Inappropriate or excessive use of cursors. Especially when a single query would suffice.
- Incorrectly assuming that triggers fire once per row affected in multi-row updates.

Determine clear division of responsibility, and strive to use the appropriate tool to solve each problem.