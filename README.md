# datahaus
A simple chat bot written in Python. Datahaus uses a MySQL database to store 
responses to a given sentence subject. It requires NLTK to be installed on the
system. NLTK ([Natural Language Tool Kit](http://www.nltk.org/)) is used to 
determine the subject of the user's input so that a logical response can be 
selected from the database.

As a very simple example, lets say your database has a key of "cats" with a
value of "I really like cats. I often collect them." The conversation would look
something like this:
```
Query> what do you think of cats?
I really like cats. I often collect them.
```

## How to create the MySQL table
The default table datahaus uses is a simple key-value setup, which can be
described as follows:
```SQL
+-------+---------------+------+-----+---------+-------+
| Field | Type          | Null | Key | Default | Extra |
+-------+---------------+------+-----+---------+-------+
| k     | varchar(256)  | YES  |     | NULL    |       |
| v     | varchar(1024) | YES  |     | NULL    |       |
+-------+---------------+------+-----+---------+-------+
```

It can be created using the following create script:
```SQL
CREATE TABLE `key_value_pairs` (
  `k` varchar(256) DEFAULT NULL,
  `v` varchar(1024) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
```

## How to teach datahaus
Datahaus is only as smart as the database it uses to get its responses. By
default your database will be empty and you'll need to teach it. There are
several different ways to teach it, so just follow along with the example
below and you can figure it out:
```
Query> learn cats: I really like cats. I often collect them.

Query> set dogs = I like dogs. They get your sense of humor. Nobody else does that.

Query> what do you think of dogs?
 I like dogs. They get your sense of humor. Nobody else does that.

Query> I hate cats.
 I really like cats. I often collect them.
```
"Set" and "learn" do the same thing.

## How to see what datahaus knows about a subject
To display a list of possible responses to a given input, you can use the "get"
keyword, which will cause datahaus to display a list of possible responses, as
follows:
```
Query> get cats
----
cats:  I really like cats. I often collect them.
```
The "list" keyword is another way to use the "get" keyword.

## How to make datahaus forget
To delete a response from the database, you can use the "forget" keyword, as
follows:
```
Query> forget cats
----
cats:  I really like cats. I often collect them.

Delete above rows? (Y/N): y
Query> get cats

Query> what do you think of cats?
I don't have anything to say about that.

```
The "del" and "drop" keywords are other ways to use the "forget" keyword.  
