---
layout: post
title: "Best way to read  large (2.5GB <) text file and store in CouchDB."
date: 2015-07-12 00:00:00
categories: tech post
---

I need to do some parsing and storing into CouchDB of large (> 2.5gb) text file in NodeJS. I have faced a huge problem with parsing and storing the content to due to memmory issue, because if you read the entire file in NodeJS it will abort due to insufficient memmory. So I have take the chalenge and researched in various solution in web, finally I have created a script with NodeJS language.

All third party librares are not suitable for my needs since the processed the files not line by line ore read the entire file into memmory. the following script can parse very large file line by line using stream and pipe. For testing I used a file size 2.6gb with total 2.5core json records. Ram usage did not exeed 2% of 2GB.



{% highlight javascript %}
var fs = require('fs');
var stream = require('stream');
var es = require('event-stream');
var nano = require('nano')('http://localhost:5984');
var text_file = "/Users/home/demo/file_dir/text_file.txt";

/* Function for create a database named as 'demo_data' in CouchDB*/
nano.db.create('demo_data', function(err, body){
    if(err){
        if(err.error == "file_exists"){
	    console.log("Database 'demo-data' already exists.");
	    return;
	}else{
	    console.log('Database creation error', err);
    	}
    } else {
        console.log('Database demo-data created!.')
    }
});

/* Store record into CouchDB.*/
fuction insert_record_into_db(params, record, total_records, start,
    callback){
    db2.insert(params, function(err, body){
        var end = new Date().getTime();
        var time = end - start;
	if(err) {
	   console.log('Errror: '+ key, err.message)
	   callback;
	   return;
	}else{
	   console.log('You have inserted record: ', key, "record no: " ,
	   ++record, "from Total recodrds", total_records, "Time Elapsed: "
	   + time + "mseconds.");
	   callback;
	}
    });
}

/* Read line by line.*/
function read_line_by_line(file, total_records){
    var record = 0;
    var start = new Date().getTime();
    var line_no = 1;
    s= fs.createReadStream(file)
       .pipe(es.split())
       .pipe(es.mapSync(function(line){
	    // pause the readstream
	    s.pause();
            line_no += 1;
	    (function(){
                // process line here and call s.resume() when ready
	       if(line){
	           var json = JSON.parse(line.slice(line.indexOf("{"),
		                 (line.lastIndexOf("}") + 1)));
		   var key = json["key"];
		   var type = json["type"]["key"];
		   var last_modified = json["last_modified"]["value"];
		   var revision = json["revision"];
		   var parms =  {_id: key, type: type,
          modified_at:last_modified, rev:revision, json:json};
		   insert_record_into_db(parms, record, total_records, start,
		                            s.resume());
	       }
	   })();
	}).on('error', function(){
	     console.log('Error while reading file.');
	}).on('end', function(){
             console.log("Reading entirefile.");
	})
    );
}

/* Calculating the total number of records and then storing the
   records line by line to CouchDB. */
var total_records = 1
s = fs.createReadStream(text_file)
    .pipe(es.split())
    .pipe(es.mapSync(function(line){
        // pause the readstream
        s.pause();
        total_records += 1;
        (function(){
            // resume the readstream
            s.resume();
        })();
    })
    .on('error', function(){
        console.log('Error while reading file.');
    })
    .on('end', function(){
        read_line_by_line("file.txt", total_records)
    })
);
{% endhighlight %}
