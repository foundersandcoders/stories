In our first week after finishing the eight week foundation of Founders & Coders, we (Asim, Greg, Per) decided to try and build and launch a meaningful product in just five days. On the Friday, after some frantic last-minute adjustments, we shipped our project — [Keyword King](http://www.keywordking.co/). In this post, we’ll talk a bit about what we built and share some of the things we learned from the experience.

## The Product
We wanted something that was simple enough so a working end-to-end alpha version could be made in the five days. Keyword King is a **web application that helps app developers to attract more users by improving their search visibility.** It analyses your app’s keywords and identifies the under-performing ones that you should change ASAP.

We tried to keep the user journey super simple.

A visitor to Keyword King would:

### Enter their app’s name.
Copy and paste in their keywords.
Get their keyword report with the option to have it emailed to them as a PDF.
The whole process shouldn’t take much more than one minute.

![alt text](/keyword_king/images/1.png)
![alt text](/keyword_king/images/2.png)
![alt text](/keyword_king/images/3.png)

### Technologies Used
Because Founders and Coders is a Javascript- heavy course, our backend is built in Node.js using the hapi framework. We have a mongoDB database and are using Joi for data validation. The PDF functionality was done using the Webshot module and we used the Mandrill API to handle emailing reports out to users.

The frontend is more lightweight. We didn’t go with a templating framework so it’s in plain HTML and uses jQuery and vanilla CSS to manage the app’s presentation. There’s a bit of d3.js that powers the pieces of data visualisation in the report and we’re using AJAX to make requests to the iTunes API from the client side.


## Challenges

### Webshot module — making compromises
One of the first things we tackled was how to attach a PDF report to an email and send it to each person. We wanted to make the emailed report different to the one instantly available on the site. This would mean the application could evolve to have a nice pro report available as an ‘upsell’. The logical thing would be to have the emailed report rendered server side, as the user shouldn’t be able to navigate to it. But after some research we discovered the Webshot module. This works by taking a screenshot of a URL and saving it down as a PDF. This did so much of what we wanted out-the-box… but meant we’d have to create the report in the browser on a dummy URL that the user would never gain access to. We were also wary that d3.js renders client side, so made the decision to sacrifice some efficiency and run with using the Webshot module.

### Rate limiting
We got the data request from the iTunes API working on Monday. But at some point on Wednesday it just stopped working. Uh oh. Turns out we had angered the Apple gods by making too many requests (unhelpfully, Apple don’t disclose what the limits are). We didn’t plan to store keyword data but this forced our hand as the app was clearly unscalable otherwise. Originally, the app would make a request for each keyword in the user’s list, which was simpler to code but not particularly efficient. In the revised version, the app first looks in the database and retrieves the data for all the keywords it can. Only then will it make new requests to the iTunes API. The information of any newly searched keywords is also stored into the database for future use.

This only becomes a big efficiency win when there is significant overlap in peoples’ searches, i.e. when there are many thousands of users. When there are tens or hundreds, there isn’t too much likelihood that people will be searching the same keywords. This is the reason why we weren’t going to worry about it for this 5-day MVP. However, the revised architecture has the advantage of us being able to make a big list of words and steadily make the requests and store the data. Over a couple of days we accumulated over 10,000 keywords in our database which will help to reduce the load on the iTunes API from launch day.

### Databases
A knock-on effect of storing tens of thousands of keywords was that we became worried about the size of our burgeoning database. We reorganised how we were storing our data and stripped it right back to only what was essential.

### The Never Ending To-Do List
One thing we didn’t fully appreciate was the tendency for a task list to keep on growing. At the beginning of the week we were even entertaining thoughts of finishing the core project early and having time to add more features (haha, as if). But as we progressed through the week, at times it seemed that tackling one task would simply spawn several more, pushing the finishing line further and further away. In future, we’ll factor in a more generous portion of contingency time.


## Extending the Project

### Better Frontend
In the end, we only spent maybe 1.5 days on the frontend. We threw everything into an index.html then wrote a bunch of jQuery to handle the transitions between groups of elements as the user moves through the site. With more time we’d go for a more sophisticated setup that would allow us to expand the site easier. On the Founders & Coders course we spent a week using React.js and Jade respectively, so those would have been likely candidates to manage the frontend better. Also, without a proper designer on board, we conceded that it would never be an aesthetic marvel. So we just tried to keep things simple.

There were also a host of features such as a progress bar, a percentage value on the pie chart and many more things which we ultimately had to let go (the report especially could be spruced up a fair bit).

### Giving visitors demo data
We were aware that the app was for quite a specific audience — people who already had an app in the app store. There would be others who may interested in trying out Keyword King quickly, but without owning an app it’s a bit awkward. We could have provided some functionality that chose a demo app and list of keywords to make peoples’ lives easier. In its current state, such people may end up bouncing before reaching the end.

### Suggesting New Keywords
Right now, the report identifies if you have a problem with your keywords. But it doesn’t actively solve that problem for you. It would be much better if we could also say “hey, you should drop these 7 keywords and replace them with these ones instead”.

We could do things that existing tools do, such as:

1. look at competing apps and what keywords work for them.
1. mine your app’s reviews for frequently mentioned words.
1. scan tweets about your app.

It would be interesting if anyone has any suggestions on an innovative way to do this.


## Overall
On the whole it was a really enjoyable week. The prospect of shipping a product out into the real world made us think a lot more about our work. In previous weeks on the course we’d demo the week’s work on localhost and that was that. This time, we tackled things like getting it hosted on a proper domain, setting up Google Analytics, cleaning up UX bugs, making the app more efficient, metadata for better SEO, and — most importantly — choosing a favicon. Despite its compromises, we were thankful that we could release the project on the deadline. Hopefully it will be one of many to come.

Thanks for reading. Feel free to check it out here, and any comments/feedback would be appreciated.

Greg, Per & Asim

Founders & Coders — Jan 15’ Cohort

