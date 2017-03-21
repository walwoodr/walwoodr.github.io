---
layout: post
title:  "Find the unfindable category"
date:   2017-03-21 19:05:26 +0000
---

### The Problem
On reading the requirements for the Command Line Interface project, a project immediately popped to mind. A blog I enjoyed reading while I was planning my wedding also frequently posts insightful, thoughtful essays about non-wedding-related topics, but after the wedding, I just couldn't handle seeing 8 posts a day in my RSS feed about weddings and to have to dig through them for the one or two posts a week that weren't full of dress choices and family drama.* They categorize the posts! Surely,* I thought, *there's a way to just look at the non-wedding posts.* But, in fact, the interface offered master list of post categories, so while I could look at a list of posts if I knew the category I wanted, I would have had to dig through several posts to find a category. That wasn't going to happen. 

Enter programming. 

### Proof of Concept
To start, I looked at the site to verify that the information I wanted to scrape was available. I found a list of articles in the "Marriage Essays" category and pulled up developer tools. Then the CMS offered up its bounty -- not only were the articles tagged with their categories, but each time a link to an article with a category appeared, that link too was tagged with a css class  'category-name'. A little more digging revealed that the The tail end of the category css class was the same as the url being used by the CMS to create category list pages. Mission impossible became mission possible.  

### Scraping 
I began writing my CLI by creating a scraping class, and defining the specific pieces of information that I wanted to scrape.
* Generate a list of categories
* Generate a list of articles per category
* Extract information about each article

To generate the list of categories, I wrote a Scraper class method to grab all of the css classes that were used on links in a list of articles in a "Marriage Essays" category. I then cleaned up those css classes so that I was left only with css classes that began with "category-", stripped the "category-" off of the css class name and passed that class name as an initializing variable to a new Category class in my application, making sure not to duplicate classes. I initialized the Category object with an empty array of articles, a URL (the css class name) and a name (the css class name Titelized). 

To generate the list of articles per category, I built another Scraper class method. I started by using the Category object's URL attribute to generate a URL from which to scrape articles. The website used the same css class as a URL namespace for listing articles in that category, which made it easy to start. The scraper iterated over the links on that site to extract the title and URL of each article. This was one of the areas I majorly refactored, so here's a sneak peek into both parts. 
* **Initial build** -- Initially on building the article list scraper, I used the article information to build out an array of hashes that contained the title (for display) and the URL (for further scraping).
* **Refactor** -- I refactored this code so that rather than adding a hash to an array when it iterated over the links, instead it instantiated an Article object including the attributes that were already being scraped (the title, the URL and the category of the list being searched.)

To generate information about the article, I again built a Scraper class method. This method took an argument of an article's URL and created a hash containing article attributes based on the article page (title, author, categories, and a 400-character article blurb). The method returned that hash that could then be used as an argument in an Article object instantiation. 

### The Classes
With all of these scraper methods working, I used their outcomes to build out the Article and Category objects. The Category object could be instantiated from the scraper that generated lists of objects, had article objects added to it's articles attribute through generating a list of articles per category, and could be instantiated with a new category *and* could have an article objected added to its articles attribute in the article scraper. 

The article object was untouched throughout generating a list of categories, had a stub article created when the category list was scraped, and was fully fleshed out when the article was scraped. 

### The CLI Interface 
From this point, I built a CLI that scrapes the article list for categories and displays the category list for selection by number. The CLI then displays 10 articles per category, with the ability to scroll up through the articles. This is another of the areas I majorly refactored, so here's another peek inside the works: 
* **Initial build** -- Initially on building the article list viewer, the scraper iterated through all possible pages of articles before displaying a single article. This led to substantial lag in loading the initial 10 articles, as well as every subsequent 10 articles.
* **Refactor** -- I refactored this code with performance in mind. After determining that each page of articles is 66 articles lonog, I set up a conditional chain in the Scraper class method to only scrape the page(s) that would be needed for the CLI article list that was currently being presented. I also built a conditional into the display method that only calls the scraper when the page will need information about articles that hadn't yet been scraped or instantiated. It's much faster! 

After the CLI has displayed the list of articles, the user can choose an article to see more information about. Once the article is chosen, the Scraper article scraping class method scrapes the article, and the CLI displays the information about the article. 

After everything was put togehter and working, my User Experience Design background also inspired me to build validation into the CLI gets using until loops to verify that the input is valid and prompt with more information if the input isn't valid. I also added some colors to the CLI interface via the Colorize Gem. 
