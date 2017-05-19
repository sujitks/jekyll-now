---
layout: post
title: Lucene the search engine
---

### Introduction
Organization of the information in different categories and accessing them quickly is always interesting subject. We always keep storing useful information in database, documents, and other different storage medium.

Now when we have cheaper storage device and lots of sources for information everyday it becomes difficult to organize the data and access them as and when required.

All of the IT Systems are based on information collected over the period of time. To access such large data spread in different form of storage medium such as documents, database, texts stored in database. Faster searching the required information was always a challenge for IT professionals.

Full text search is one of the favorite techniques for Architects for searching text. There are many example of it. Most of the search engines are based on full text searching. Some time it is called as Free Text searching because user don’t need to type their queries in particular structured manner.

There are many Full Text Solutions available in the market from almost all major software vendors. Microsoft Index server, Oracle Text, Sql Server, Microsoft Lookout are few of the full text searching solution.

Lucene is open source indexing and searching solution supported by Apache Software Foundation using Java as the programming language. First version of Lucene (0.01) was released in March 2000 (8 years ago) from the SourceForge. First apache Jakarta release was released in June 2002 (version 1.2).

### Overview
Lucene is a high performance and scalable information retrieval open source framework. This is being used by many famous organizations for their search solution. Microsoft Lookout is also based on Lucene.net (.net port of Lucene).

Some of the organizations which use Lucene are following:
o Encyclopedia Britannica
o FedEx
o Mayo Clinic
o Hewlet Packard
o Midgard
o LendingClub.com
o Wikipedia, MediaWiki, Joost, ISOHunt and many more…

Lucene was initiated by Doug Cutting, the creator of first open source World Wide Web search engine (www.nutch.org). Initially this was developed in Java and later it is ported to different programming languages as well.

Since all language port of Lucene uses same algorithm of creating index, index created by Lucene (Java) could be used by lucene.net or its port to C, Python, and Ruby etc.

It is very surprising to know that many oracle DBA and users wrote in different forums that Lucene works well than Oracle Text for them. There is another open source initiative is going on for Lucene support inside Oracle JVM as OJVMDirectory.

As oracle supports java and has got built in JVM in oracle database entire Lucene can be deployed inside oracle using loadjava utility.

I have not got opportunity to work with OJVMDirecotry initiative but it sounds very interesting. Lucene hosted inside oracle and you can build Lucene based indexes on the tables and later on these indexes can be used with combination of other Sql or pl/Sql code. J

### Getting started with Lucene

For using Lucene in the application we have to first build index by using different classes and APIs. All samples give here are based on Lucene.net (dot net port of Lucene).

Creating Lucene index is quite easy and there are many options to build customized index as per the requirement.

Following is the example of creating and using Lucene index for Addresses table of a database. In a particular scenario where addresses table has millions of records, it is very difficult to search a particular address in it. There is a text field in the table to store the address (combination of street, house number, house name, city, post code, town and any other attribute of an address).
For example following are the same address written in different format:

1, Street, House Name, Near Xyz, City, Town, Country, Postcode,
1, House name, near XYZ City, Street, Town, City, country, Postcode

And may be incomplete information
1, House Name, Street, Town, Postcode

For all of the above scenarios searching the exact address from the millions of the record is quite difficult.

Following example build index for Addresses in the database for its Address and address id field.

Building the Lucene index from database table

Listing 1 this is assumed that a dataset is populated with the result
```
using System;
using System.Collections.Generic;
using System.Text;
using System.IO;
using System.Data;
using Oracle.DataAccess.Client;
using Lucene.Net.Index;
using Lucene.Net.Analysis;
using Lucene.Net.Documents;
using Lucene.Net.Store;
using System.Xml;

namespace CCSDbIndexer
{
public class Program
{

[STAThread]
public static void Main(string[] args)
{

//parse the command line arguments

parseCommandLine(Environment.CommandLine);

index();
}

private static void index()
{
FileInfo indexLocation = new FileInfo(Path.Combine(Environment.CurrentDirectory, “Index”));

IndexWriter writer =
new IndexWriter(indexLocation, new Lucene.Net.Analysis.Standard.StandardAnalyzer(), true);

writer.SetMergeFactor(100);
writer.SetMaxBufferedDocs(1000);
writer.SetMaxMergeDocs(1000);

writer.SetMaxFieldLength(400);

Oracle.DataAccess.Client.OracleConnection con = new Oracle.DataAccess.Client.OracleConnection(connectionString);

DataSet ds = getDataset(…);

while (rowReturned(ds))
{
//index the rows…
indexDataset(ds, writer);
ds = getDataset(…);
}

writer.Optimize();

writer.Close();
}

```

Note: This is a very small example of building lucene index from a dataset. There are many API (public classes and methods) available to be used to control the building of index. SetMergeFactor, SetMaxBufferedDocs, SetMaxMergeDocs are some of the instance method of IndexWriter class which is can be manipulated for faster indexing of the documents. As we know that there is always trade off in hardware and performance. So if you put more RAM and use multiple processor you can use all of the resource for building large indexes.

```
private static void indexDataset(DataSet ds, IndexWriter writer)
{

foreach (DataTable dt in ds.Tables)
{
DataColumnCollection columns = dt.Columns;

foreach (DataRow row in dt.Rows)
{
Document doc = new Document();
foreach (DataColumn col in columns)
{
//check whether field is avialable in our collection

if (fieldAttribute.Count > 0)
{
if (fieldAttribute.ContainsKey(col.ColumnName.ToUpper()))
{
IndexFieldProperty field = fieldAttribute[col.ColumnName.ToUpper()];

doc.Add(new Field(field.fieldName, row[col.ColumnName].ToString(), Field.Store.YES, (field.canTokenized) ? Field.Index.TOKENIZED : Field.Index.UN_TOKENIZED));
}
}
}
if (doc.GetFieldsCount() > 0)
writer.AddDocument(doc);
}
}

}
```
Note: Have you noticed different parameters of the Field constructor? These are attributes of a field in a Document (Lucene Document) collection of documents formed a lucene index. By specifying the different attribute of Field we can tell lucene framework to store the information in different ways. For example we may want to tokenise a big string or we want to store the entire string as a single field. We can use different Analyser which are useful for handling punctuations, number or language specific words… etc.

### Searching the Lucene index

Searching the information from Lucene index is very easy. There are various options available to get the result using the filter criteria, sorting etc. There are different analyzers available to be used with and one can write a custom analyzer for specific language or need.

Listing 2 Searching the Addresses from Lucene Index

```
Public static Addresses SearchAddresses()
{

Lucene.Net.Index.IndexReader reader = Lucene.Net.Index.IndexReader.Open(addressIndexFolder());

Lucene.Net.Search.Searcher searcher = new Lucene.Net.Search.IndexSearcher(reader);
Lucene.Net.Analysis.Analyzer analyser = new Lucene.Net.Analysis.Standard.StandardAnalyzer();

//create a query parser by specifying the field name…
Lucene.Net.QueryParsers.QueryParser parser = new Lucene.Net.QueryParsers.QueryParser(“OneLineAddress”, analyser);

Lucene.Net.Search.Query query = parser.Parse(searchString);

Lucene.Net.Search.Hits hits = searcher.Search(query, Lucene.Net.Search.Sort.RELEVANCE);

int total = hits.Length();
total = (total < 50) ? total : 50;
//collection of address
Addresses addressSummary = new Addresses();

for (int i = 0; i < total; i++)
{
Lucene.Net.Documents.Document doc = hits.Doc(i);
Address summary = new Address();
Summary.AdressID = (doc.Get(“AddressId”);
Summary.OneLineAddress = doc.Get(“OneLineAddress”);
addressSummarys.Add(summary);

}
return Addresses;
}
```
Note: Similar to indexing there are various methods available to control the search behavior. We can use different analyzers to for treating punctuation, white spaces, numbers etc differently. There are various options available with Searcher class to get the searched result in specific order.

### Summary

Lucene is a very powerful and robust search framework which is being used by many organizations since years. There are many language ports available to be used.

In a scenario where it becomes very difficult to provide a cost effective and robust search solution to search and match the records on the basis of percentage match of different textual information, Lucene helps a lot and also appreciated by everyone.

Performance is tremendous and I must say that if you like google style search and index solution for your enterprise, Try Lucene. And no wonder if you will love using and recommending it.

There are various website, search engines and applications which are already using Lucene underneath. This is very useful tool/framework for building enterprise application with index and search capabilities, document management system, information repository system, Mobile application (please note size of entire Lucene.net assembly after compilation form is less than 300 KB).

### References

[Jakarta Apache site](http://lucene.apache.org/)

[Wikipedia](http://en.wikipedia.org/wiki/Lucene/)

[Free Search blog](www.lucene.com)

[Lucene.net](http://incubator.apache.org/Lucene.net)

[Marcelo Ochoa Blog](http://marceloochoa.blogspot.com/2007/09/turbocharge-your-oracle-jvm.html (Lucene domain index in oracle)

[Lucene in action](http://www.manning.com/hatcher2/) ISBN: 1932394281.

There are loads of materials available for the topic if you search in any popular search engine.
