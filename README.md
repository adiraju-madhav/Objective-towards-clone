# Objective-towards-clone
{  "cells": [   {    "cell_type": "markdown",    "metadata": {},    "source": [     "# Guide to Web Scraping\n",     "\n",     "Let's get you started with web scraping and Python. Before we begin, here are some important rules to follow and understand:\n",     "\n",     "1. Always be respectful and try to get premission to scrape, do not bombard a website with scraping requests, otherwise your IP address may be blocked!\n",     "2. Be aware that websites change often, meaning your code could go from working to totally broken from one day to the next.\n",     "3. Pretty much every web scraping project of interest is a unique and custom job, so try your best to generalize the skills learned here.\n",     "\n",     "OK, let's get started with the basics!\n",     "\n",     "## Basic components of a WebSite\n",     "\n",     "### HTML\n",     "HTML stands for  Hypertext Markup Language and every website on the internet uses it to display information. Even the jupyter notebook system uses it to display this information in your browser. If you right click on a website and select \"View Page Source\" you can see the raw HTML of a web page. This is the information that Python will be looking at to grab information from. Let's take a look at a simple webpage's HTML:\n",     "\n",     "    &lt;!DOCTYPE html>  \n",     "    &lt;html>  \n",     "        &lt;head>\n",     "            &lt;title>Title on Browser Tab&lt;/title>\n",     "        &lt;/head>\n",     "        &lt;body>\n",     "            &lt;h1> Website Header &lt;/h1>\n",     "            &lt;p> Some Paragraph &lt;/p>\n",     "        &lt;body>\n",     "    &lt;/html>"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "Let's breakdown these components.\n",     "\n",     "Every &lt;tag> indicates a specific block type on the webpage:\n",     "\n",     "    1.&lt;DOCTYPE html> HTML documents will always start with this type declaration, letting the browser know its an HTML file.\n",     "    2. The component blocks of the HTML document are placed between &lt;html> and &lt;/html>.\n",     "    3. Meta data and script connections (like a link to a CSS file or a JS file) are often placed in the &lt;head> block.\n",     "    4. The &lt;title> tag block defines the title of the webpage (its what shows up in the tab of a website you're visiting).\n",     "    5. Is between &lt;body> and &lt;/body> tags are the blocks that will be visible to the site visitor.\n",     "    6. Headings are defined by the &lt;h1> through &lt;h6> tags, where the number represents the size of the heading.\n",     "    7. Paragraphs are defined by the &lt;p> tag, this is essentially just normal text on the website.\n",     "\n",     "    There are many more tags than just these, such as &lt;a> for hyperlinks, &lt;table> for tables, &lt;tr> for table rows, and &lt;td> for table columns, and more!"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "### CSS\n",     "\n",     "CSS stands for Cascading Style Sheets, this is what gives \"style\" to a website, including colors and fonts, and even some animations! CSS uses tags such as **id** or **class** to connect an HTML element to a CSS feature, such as a particular color. **id** is a unique id for an HTML tag and must be unique within the HTML document, basically a single use connection. **class** defines a general style that can then be linked to multiple HTML tags. Basically if you only want a single html tag to be red, you would use an id tag, if you wanted several HTML tags/blocks to be red, you would create a class in your CSS doc and then link it to the rest of these blocks."    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "### Scraping Guidelines\n",     "\n",     "Keep in mind you should always have permission for the website you are scraping! Check a websites terms and conditions for more info. Also keep in mind that a computer can send requests to a website very fast, so a website may block your computer's ip address if you send too many requests too quickly. Lastly, websites change all the time! You will most likely need to update your code often for long term web-scraping jobs."    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "## Web Scraping with Python\n",     "\n",     "There are a few libraries you will need, you can go to your command line and install them with conda install (if you are using anaconda distribution), or pip install for other python distributions.\n",     "\n",     "    conda install requests\n",     "    conda install lxml\n",     "    conda install bs4\n",     "    \n",     "if you are not using the Anaconda Installation, you can use **pip install** instead of **conda install**, for example:\n",     "\n",     "    pip install requests\n",     "    pip install lxml\n",     "    pip install bs4\n",     "    \n",     "Now let's see what we can do with these libraries.\n",     "\n",     "----"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "### Example Task 0 - Grabbing the title of a page\n",     "\n",     "Let's start very simple, we will grab the title of a page. Remember that this is the HTML block with the **title** tag. For this task we will use **www.example.com** which is a website specifically made to serve as an example domain. Let's go through the main steps:"    ]   },   {    "cell_type": "code",    "execution_count": 51,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "import requests"    ]   },   {    "cell_type": "code",    "execution_count": 52,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "# Step 1: Use the requests library to grab the page\n",     "# Note, this may fail if you have a firewall blocking Python/Jupyter \n",     "# Note sometimes you need to run this twice if it fails the first time\n",     "res = requests.get(\"http://www.example.com\")"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "This object is a requests.models.Response object and it actually contains the information from the website, for example:"    ]   },   {    "cell_type": "code",    "execution_count": 53,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "requests.models.Response"       ]      },      "execution_count": 53,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "type(res)"    ]   },   {    "cell_type": "code",    "execution_count": 54,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "'&lt;!doctype html>\\n&lt;html>\\n&lt;head>\\n    &lt;title>Example Domain&lt;/title>\\n\\n    &lt;meta charset=\"utf-8\" />\\n    &lt;meta http-equiv=\"Content-type\" content=\"text/html; charset=utf-8\" />\\n    &lt;meta name=\"viewport\" content=\"width=device-width, initial-scale=1\" />\\n    &lt;style type=\"text/css\">\\n    body {\\n        background-color: #f0f0f2;\\n        margin: 0;\\n        padding: 0;\\n        font-family: -apple-system, system-ui, BlinkMacSystemFont, \"Segoe UI\", \"Open Sans\", \"Helvetica Neue\", Helvetica, Arial, sans-serif;\\n        \\n    }\\n    div {\\n        width: 600px;\\n        margin: 5em auto;\\n        padding: 2em;\\n        background-color: #fdfdff;\\n        border-radius: 0.5em;\\n        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);\\n    }\\n    a:link, a:visited {\\n        color: #38488f;\\n        text-decoration: none;\\n    }\\n    @media (max-width: 700px) {\\n        div {\\n            margin: 0 auto;\\n            width: auto;\\n        }\\n    }\\n    &lt;/style>    \\n&lt;/head>\\n\\n&lt;body>\\n&lt;div>\\n    &lt;h1>Example Domain&lt;/h1>\\n    &lt;p>This domain is for use in illustrative examples in documents. You may use this\\n    domain in literature without prior coordination or asking for permission.&lt;/p>\\n    &lt;p>&lt;a href=\"https://www.iana.org/domains/example\">More information...&lt;/a>&lt;/p>\\n&lt;/div>\\n&lt;/body>\\n&lt;/html>\\n'"       ]      },      "execution_count": 54,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "res.text"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "____\n",     "Now we use BeautifulSoup to analyze the extracted page. Technically we could use our own custom script to loook for items in the string of **res.text** but the BeautifulSoup library already has lots of built-in tools and methods to grab information from a string of this nature (basically an HTML file). Using BeautifulSoup we can create a \"soup\" object that contains all the \"ingredients\" of the webpage. Don't ask me about the weird library names, I didn't choose them! :)"    ]   },   {    "cell_type": "code",    "execution_count": 55,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "import bs4"    ]   },   {    "cell_type": "code",    "execution_count": 56,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "soup = bs4.BeautifulSoup(res.text,\"lxml\")"    ]   },   {    "cell_type": "code",    "execution_count": 57,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "&lt;!DOCTYPE html>\n",        "&lt;html>\n",        "&lt;head>\n",        "&lt;title>Example Domain&lt;/title>\n",        "&lt;meta charset=\"utf-8\"/>\n",        "&lt;meta content=\"text/html; charset=utf-8\" http-equiv=\"Content-type\"/>\n",        "&lt;meta content=\"width=device-width, initial-scale=1\" name=\"viewport\"/>\n",        "&lt;style type=\"text/css\">\n",        "    body {\n",        "        background-color: #f0f0f2;\n",        "        margin: 0;\n",        "        padding: 0;\n",        "        font-family: -apple-system, system-ui, BlinkMacSystemFont, \"Segoe UI\", \"Open Sans\", \"Helvetica Neue\", Helvetica, Arial, sans-serif;\n",        "        \n",        "    }\n",        "    div {\n",        "        width: 600px;\n",        "        margin: 5em auto;\n",        "        padding: 2em;\n",        "        background-color: #fdfdff;\n",        "        border-radius: 0.5em;\n",        "        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);\n",        "    }\n",        "    a:link, a:visited {\n",        "        color: #38488f;\n",        "        text-decoration: none;\n",        "    }\n",        "    @media (max-width: 700px) {\n",        "        div {\n",        "            margin: 0 auto;\n",        "            width: auto;\n",        "        }\n",        "    }\n",        "    &lt;/style>\n",        "&lt;/head>\n",        "&lt;body>\n",        "&lt;div>\n",        "&lt;h1>Example Domain&lt;/h1>\n",        "&lt;p>This domain is for use in illustrative examples in documents. You may use this\n",        "    domain in literature without prior coordination or asking for permission.&lt;/p>\n",        "&lt;p>&lt;a href=\"https://www.iana.org/domains/example\">More information...&lt;/a>&lt;/p>\n",        "&lt;/div>\n",        "&lt;/body>\n",        "&lt;/html>"       ]      },      "execution_count": 57,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "soup"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "Now let's use the **.select()** method to grab elements. We are looking for the 'title' tag, so we will pass in 'title'\n"    ]   },   {    "cell_type": "code",    "execution_count": 7,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "[&lt;title>Example Domain&lt;/title>]"       ]      },      "execution_count": 7,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "soup.select('title')"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "Notice what is returned here, its actually a list containing all the title elements (along with their tags). You can use indexing or even looping to grab the elements from the list. Since this object it still a specialized tag, we cna use method calls to grab just the text."    ]   },   {    "cell_type": "code",    "execution_count": 8,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "title_tag = soup.select('title')"    ]   },   {    "cell_type": "code",    "execution_count": 9,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "&lt;title>Example Domain&lt;/title>"       ]      },      "execution_count": 9,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "title_tag[0]"    ]   },   {    "cell_type": "code",    "execution_count": 11,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "bs4.element.Tag"       ]      },      "execution_count": 11,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "type(title_tag[0])"    ]   },   {    "cell_type": "code",    "execution_count": 12,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "'Example Domain'"       ]      },      "execution_count": 12,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "title_tag[0].getText()"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "### Example Task 1 - Grabbing all elements of a class\n",     "\n",     "Let's try to grab all the section headings of the Wikipedia Article on Grace Hopper from this URL: https://en.wikipedia.org/wiki/Grace_Hopper"    ]   },   {    "cell_type": "code",    "execution_count": 13,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "# First get the request\n",     "res = requests.get('https://en.wikipedia.org/wiki/Grace_Hopper')"    ]   },   {    "cell_type": "code",    "execution_count": 14,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "# Create a soup from request\n",     "soup = bs4.BeautifulSoup(res.text,\"lxml\")"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "Now its time to figure out what we are actually looking for. Inspect the element on the page to see that the section headers have the class \"mw-headline\". Because this is a class and not a straight tag, we need to adhere to some syntax for CSS. In this case"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "&lt;table>\n",     "\n",     "&lt;thead >\n",     "&lt;tr>\n",     "&lt;th>\n",     "&lt;p>Syntax to pass to the .select() method&lt;/p>\n",     "&lt;/th>\n",     "&lt;th>\n",     "&lt;p>Match Results&lt;/p>\n",     "&lt;/th>\n",     "&lt;/tr>\n",     "&lt;/thead>\n",     "&lt;tbody>\n",     "&lt;tr>\n",     "&lt;td>\n",     "&lt;p>&lt;code>soup.select('div')&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;td>\n",     "&lt;p>All elements with the &lt;code>&amp;lt;div&amp;gt;&lt;/code> tag&lt;/p>\n",     "&lt;/td>\n",     "&lt;/tr>\n",     "&lt;tr>\n",     "&lt;td>\n",     "&lt;p>&lt;code>soup.select('#some_id')&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;td>\n",     "&lt;p>The HTML element containing the &lt;code>id&lt;/code> attribute of &lt;code>some_id&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;/tr>\n",     "&lt;tr>\n",     "&lt;td>\n",     "&lt;p>&lt;code>soup.select('.notice')&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;td>\n",     "&lt;p>All the HTML elements with the CSS &lt;code>class&lt;/code> named &lt;code>notice&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;/tr>\n",     "&lt;tr>\n",     "&lt;td>\n",     "&lt;p>&lt;code>soup.select('div span')&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;td>\n",     "&lt;p>Any elements named &lt;code>&amp;lt;span&amp;gt;&lt;/code> that are within an element named &lt;code>&amp;lt;div&amp;gt;&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;/tr>\n",     "&lt;tr>\n",     "&lt;td>\n",     "&lt;p>&lt;code>soup.select('div &amp;gt; span')&lt;/code>&lt;/p>\n",     "&lt;/td>\n",     "&lt;td>\n",     "&lt;p>Any elements named &lt;code class=\"literal2\">&amp;lt;span&amp;gt;&lt;/code> that are &lt;span>&lt;em >directly&lt;/em>&lt;/span> within an element named &lt;code class=\"literal2\">&amp;lt;div&amp;gt;&lt;/code>, with no other element in between&lt;/p>\n",     "&lt;/td>\n",     "&lt;/tr>\n",     "&lt;tr>\n",     "\n",     "&lt;/tr>\n",     "&lt;/tbody>\n",     "&lt;/table>"    ]   },   {    "cell_type": "code",    "execution_count": 16,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "[&lt;span class=\"mw-headline\" id=\"Early_life_and_education\">Early life and education&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Career\">Career&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"World_War_II\">World War II&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"UNIVAC\">UNIVAC&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"COBOL\">COBOL&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Standards\">Standards&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Retirement\">Retirement&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Post-retirement\">Post-retirement&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Anecdotes\">Anecdotes&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Death\">Death&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Dates_of_rank\">Dates of rank&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Awards_and_honors\">Awards and honors&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Military_awards\">Military awards&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Other_awards\">Other awards&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Legacy\">Legacy&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Places\">Places&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Programs\">Programs&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"In_popular_culture\">In popular culture&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Grace_Hopper_Celebration_of_Women_in_Computing\">Grace Hopper Celebration of Women in Computing&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Notes\">Notes&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Obituary_notices\">Obituary notices&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"See_also\">See also&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"References\">References&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"Further_reading\">Further reading&lt;/span>,\n",        " &lt;span class=\"mw-headline\" id=\"External_links\">External links&lt;/span>]"       ]      },      "execution_count": 16,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "# note depending on your IP Address, \n",     "# this class may be called something different\n",     "soup.select(\".toctext\")"    ]   },   {    "cell_type": "code",    "execution_count": 17,    "metadata": {},    "outputs": [     {      "name": "stdout",      "output_type": "stream",      "text": [       "Early life and education\n",       "Career\n",       "World War II\n",       "UNIVAC\n",       "COBOL\n",       "Standards\n",       "Retirement\n",       "Post-retirement\n",       "Anecdotes\n",       "Death\n",       "Dates of rank\n",       "Awards and honors\n",       "Military awards\n",       "Other awards\n",       "Legacy\n",       "Places\n",       "Programs\n",       "In popular culture\n",       "Grace Hopper Celebration of Women in Computing\n",       "Notes\n",       "Obituary notices\n",       "See also\n",       "References\n",       "Further reading\n",       "External links\n"      ]     }    ],    "source": [     "for item in soup.select(\".toctext\"):\n",     "    print(item.text)"    ]   },   {    "cell_type": "markdown",    "metadata": {},    "source": [     "### Example Task 3 - Getting an Image from a Website\n",     "\n",     "Let's attempt to grab the image of the Deep Blue Computer from this wikipedia article: https://en.wikipedia.org/wiki/Deep_Blue_(chess_computer)"    ]   },   {    "cell_type": "code",    "execution_count": 18,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "res = requests.get(\"https://en.wikipedia.org/wiki/Deep_Blue_(chess_computer)\")"    ]   },   {    "cell_type": "code",    "execution_count": 19,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "soup = bs4.BeautifulSoup(res.text,'lxml')"    ]   },   {    "cell_type": "code",    "execution_count": 20,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "image_info = soup.select('.thumbimage')"    ]   },   {    "cell_type": "code",    "execution_count": 22,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "[&lt;img alt=\"\" class=\"thumbimage\" data-file-height=\"601\" data-file-width=\"400\" decoding=\"async\" height=\"331\" src=\"//upload.wikimedia.org/wikipedia/commons/thumb/b/be/Deep_Blue.jpg/220px-Deep_Blue.jpg\" srcset=\"//upload.wikimedia.org/wikipedia/commons/thumb/b/be/Deep_Blue.jpg/330px-Deep_Blue.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/b/be/Deep_Blue.jpg 2x\" width=\"220\"/>,\n",        " &lt;img alt=\"\" class=\"thumbimage\" data-file-height=\"600\" data-file-width=\"800\" decoding=\"async\" height=\"165\" src=\"//upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Kasparov_Magath_1985_Hamburg-2.png/220px-Kasparov_Magath_1985_Hamburg-2.png\" srcset=\"//upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Kasparov_Magath_1985_Hamburg-2.png/330px-Kasparov_Magath_1985_Hamburg-2.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Kasparov_Magath_1985_Hamburg-2.png/440px-Kasparov_Magath_1985_Hamburg-2.png 2x\" width=\"220\"/>]"       ]      },      "execution_count": 22,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "image_info"    ]   },   {    "cell_type": "code",    "execution_count": 24,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "2"       ]      },      "execution_count": 24,      "metadata": {},      "output_type": "execute_result"     }    ],    "source": [     "len(image_info)"    ]   },   {    "cell_type": "code",    "execution_count": 25,    "metadata": {     "collapsed": true    },    "outputs": [],    "source": [     "computer = image_info[0]"    ]   },   {    "cell_type": "code",    "execution_count": 26,    "metadata": {},    "outputs": [     {      "data": {       "text/plain": [        "bs4.element.Tag"       ]      },      "execution_count": 26,      "metadata": {},      "output_type": "execute_result"     }    ],  
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Guide to Web Scraping\n",
    "\n",
    "Let's get you started with web scraping and Python. Before we begin, here are some important rules to follow and understand:\n",
    "\n",
    "1. Always be respectful and try to get premission to scrape, do not bombard a website with scraping requests, otherwise your IP address may be blocked!\n",
    "2. Be aware that websites change often, meaning your code could go from working to totally broken from one day to the next.\n",
    "3. Pretty much every web scraping project of interest is a unique and custom job, so try your best to generalize the skills learned here.\n",
    "\n",
    "OK, let's get started with the basics!\n",
    "\n",
    "## Basic components of a WebSite\n",
    "\n",
    "### HTML\n",
    "HTML stands for  Hypertext Markup Language and every website on the internet uses it to display information. Even the jupyter notebook system uses it to display this information in your browser. If you right click on a website and select \"View Page Source\" you can see the raw HTML of a web page. This is the information that Python will be looking at to grab information from. Let's take a look at a simple webpage's HTML:\n",
    "\n",
    "    <!DOCTYPE html>  \n",
    "    <html>  \n",
    "        <head>\n",
    "            <title>Title on Browser Tab</title>\n",
    "        </head>\n",
    "        <body>\n",
    "            <h1> Website Header </h1>\n",
    "            <p> Some Paragraph </p>\n",
    "        <body>\n",
    "    </html>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's breakdown these components.\n",
    "\n",
    "Every <tag> indicates a specific block type on the webpage:\n",
    "\n",
    "    1.<DOCTYPE html> HTML documents will always start with this type declaration, letting the browser know its an HTML file.\n",
    "    2. The component blocks of the HTML document are placed between <html> and </html>.\n",
    "    3. Meta data and script connections (like a link to a CSS file or a JS file) are often placed in the <head> block.\n",
    "    4. The <title> tag block defines the title of the webpage (its what shows up in the tab of a website you're visiting).\n",
    "    5. Is between <body> and </body> tags are the blocks that will be visible to the site visitor.\n",
    "    6. Headings are defined by the <h1> through <h6> tags, where the number represents the size of the heading.\n",
    "    7. Paragraphs are defined by the <p> tag, this is essentially just normal text on the website.\n",
    "\n",
    "    There are many more tags than just these, such as <a> for hyperlinks, <table> for tables, <tr> for table rows, and <td> for table columns, and more!"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### CSS\n",
    "\n",
    "CSS stands for Cascading Style Sheets, this is what gives \"style\" to a website, including colors and fonts, and even some animations! CSS uses tags such as **id** or **class** to connect an HTML element to a CSS feature, such as a particular color. **id** is a unique id for an HTML tag and must be unique within the HTML document, basically a single use connection. **class** defines a general style that can then be linked to multiple HTML tags. Basically if you only want a single html tag to be red, you would use an id tag, if you wanted several HTML tags/blocks to be red, you would create a class in your CSS doc and then link it to the rest of these blocks."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Scraping Guidelines\n",
    "\n",
    "Keep in mind you should always have permission for the website you are scraping! Check a websites terms and conditions for more info. Also keep in mind that a computer can send requests to a website very fast, so a website may block your computer's ip address if you send too many requests too quickly. Lastly, websites change all the time! You will most likely need to update your code often for long term web-scraping jobs."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Web Scraping with Python\n",
    "\n",
    "There are a few libraries you will need, you can go to your command line and install them with conda install (if you are using anaconda distribution), or pip install for other python distributions.\n",
    "\n",
    "    conda install requests\n",
    "    conda install lxml\n",
    "    conda install bs4\n",
    "    \n",
    "if you are not using the Anaconda Installation, you can use **pip install** instead of **conda install**, for example:\n",
    "\n",
    "    pip install requests\n",
    "    pip install lxml\n",
    "    pip install bs4\n",
    "    \n",
    "Now let's see what we can do with these libraries.\n",
    "\n",
    "----"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Example Task 0 - Grabbing the title of a page\n",
    "\n",
    "Let's start very simple, we will grab the title of a page. Remember that this is the HTML block with the **title** tag. For this task we will use **www.example.com** which is a website specifically made to serve as an example domain. Let's go through the main steps:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "import requests"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "# Step 1: Use the requests library to grab the page\n",
    "# Note, this may fail if you have a firewall blocking Python/Jupyter \n",
    "# Note sometimes you need to run this twice if it fails the first time\n",
    "res = requests.get(\"http://www.example.com\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This object is a requests.models.Response object and it actually contains the information from the website, for example:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "requests.models.Response"
      ]
     },
     "execution_count": 53,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type(res)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'<!doctype html>\\n<html>\\n<head>\\n    <title>Example Domain</title>\\n\\n    <meta charset=\"utf-8\" />\\n    <meta http-equiv=\"Content-type\" content=\"text/html; charset=utf-8\" />\\n    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1\" />\\n    <style type=\"text/css\">\\n    body {\\n        background-color: #f0f0f2;\\n        margin: 0;\\n        padding: 0;\\n        font-family: -apple-system, system-ui, BlinkMacSystemFont, \"Segoe UI\", \"Open Sans\", \"Helvetica Neue\", Helvetica, Arial, sans-serif;\\n        \\n    }\\n    div {\\n        width: 600px;\\n        margin: 5em auto;\\n        padding: 2em;\\n        background-color: #fdfdff;\\n        border-radius: 0.5em;\\n        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);\\n    }\\n    a:link, a:visited {\\n        color: #38488f;\\n        text-decoration: none;\\n    }\\n    @media (max-width: 700px) {\\n        div {\\n            margin: 0 auto;\\n            width: auto;\\n        }\\n    }\\n    </style>    \\n</head>\\n\\n<body>\\n<div>\\n    <h1>Example Domain</h1>\\n    <p>This domain is for use in illustrative examples in documents. You may use this\\n    domain in literature without prior coordination or asking for permission.</p>\\n    <p><a href=\"https://www.iana.org/domains/example\">More information...</a></p>\\n</div>\\n</body>\\n</html>\\n'"
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "res.text"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "____\n",
    "Now we use BeautifulSoup to analyze the extracted page. Technically we could use our own custom script to loook for items in the string of **res.text** but the BeautifulSoup library already has lots of built-in tools and methods to grab information from a string of this nature (basically an HTML file). Using BeautifulSoup we can create a \"soup\" object that contains all the \"ingredients\" of the webpage. Don't ask me about the weird library names, I didn't choose them! :)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "import bs4"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "soup = bs4.BeautifulSoup(res.text,\"lxml\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<!DOCTYPE html>\n",
       "<html>\n",
       "<head>\n",
       "<title>Example Domain</title>\n",
       "<meta charset=\"utf-8\"/>\n",
       "<meta content=\"text/html; charset=utf-8\" http-equiv=\"Content-type\"/>\n",
       "<meta content=\"width=device-width, initial-scale=1\" name=\"viewport\"/>\n",
       "<style type=\"text/css\">\n",
       "    body {\n",
       "        background-color: #f0f0f2;\n",
       "        margin: 0;\n",
       "        padding: 0;\n",
       "        font-family: -apple-system, system-ui, BlinkMacSystemFont, \"Segoe UI\", \"Open Sans\", \"Helvetica Neue\", Helvetica, Arial, sans-serif;\n",
       "        \n",
       "    }\n",
       "    div {\n",
       "        width: 600px;\n",
       "        margin: 5em auto;\n",
       "        padding: 2em;\n",
       "        background-color: #fdfdff;\n",
       "        border-radius: 0.5em;\n",
       "        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);\n",
       "    }\n",
       "    a:link, a:visited {\n",
       "        color: #38488f;\n",
       "        text-decoration: none;\n",
       "    }\n",
       "    @media (max-width: 700px) {\n",
       "        div {\n",
       "            margin: 0 auto;\n",
       "            width: auto;\n",
       "        }\n",
       "    }\n",
       "    </style>\n",
       "</head>\n",
       "<body>\n",
       "<div>\n",
       "<h1>Example Domain</h1>\n",
       "<p>This domain is for use in illustrative examples in documents. You may use this\n",
       "    domain in literature without prior coordination or asking for permission.</p>\n",
       "<p><a href=\"https://www.iana.org/domains/example\">More information...</a></p>\n",
       "</div>\n",
       "</body>\n",
       "</html>"
      ]
     },
     "execution_count": 57,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "soup"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now let's use the **.select()** method to grab elements. We are looking for the 'title' tag, so we will pass in 'title'\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[<title>Example Domain</title>]"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "soup.select('title')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Notice what is returned here, its actually a list containing all the title elements (along with their tags). You can use indexing or even looping to grab the elements from the list. Since this object it still a specialized tag, we cna use method calls to grab just the text."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "title_tag = soup.select('title')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<title>Example Domain</title>"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "title_tag[0]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "bs4.element.Tag"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type(title_tag[0])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'Example Domain'"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "title_tag[0].getText()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Example Task 1 - Grabbing all elements of a class\n",
    "\n",
    "Let's try to grab all the section headings of the Wikipedia Article on Grace Hopper from this URL: https://en.wikipedia.org/wiki/Grace_Hopper"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "# First get the request\n",
    "res = requests.get('https://en.wikipedia.org/wiki/Grace_Hopper')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "# Create a soup from request\n",
    "soup = bs4.BeautifulSoup(res.text,\"lxml\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now its time to figure out what we are actually looking for. Inspect the element on the page to see that the section headers have the class \"mw-headline\". Because this is a class and not a straight tag, we need to adhere to some syntax for CSS. In this case"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<table>\n",
    "\n",
    "<thead >\n",
    "<tr>\n",
    "<th>\n",
    "<p>Syntax to pass to the .select() method</p>\n",
    "</th>\n",
    "<th>\n",
    "<p>Match Results</p>\n",
    "</th>\n",
    "</tr>\n",
    "</thead>\n",
    "<tbody>\n",
    "<tr>\n",
    "<td>\n",
    "<p><code>soup.select('div')</code></p>\n",
    "</td>\n",
    "<td>\n",
    "<p>All elements with the <code>&lt;div&gt;</code> tag</p>\n",
    "</td>\n",
    "</tr>\n",
    "<tr>\n",
    "<td>\n",
    "<p><code>soup.select('#some_id')</code></p>\n",
    "</td>\n",
    "<td>\n",
    "<p>The HTML element containing the <code>id</code> attribute of <code>some_id</code></p>\n",
    "</td>\n",
    "</tr>\n",
    "<tr>\n",
    "<td>\n",
    "<p><code>soup.select('.notice')</code></p>\n",
    "</td>\n",
    "<td>\n",
    "<p>All the HTML elements with the CSS <code>class</code> named <code>notice</code></p>\n",
    "</td>\n",
    "</tr>\n",
    "<tr>\n",
    "<td>\n",
    "<p><code>soup.select('div span')</code></p>\n",
    "</td>\n",
    "<td>\n",
    "<p>Any elements named <code>&lt;span&gt;</code> that are within an element named <code>&lt;div&gt;</code></p>\n",
    "</td>\n",
    "</tr>\n",
    "<tr>\n",
    "<td>\n",
    "<p><code>soup.select('div &gt; span')</code></p>\n",
    "</td>\n",
    "<td>\n",
    "<p>Any elements named <code class=\"literal2\">&lt;span&gt;</code> that are <span><em >directly</em></span> within an element named <code class=\"literal2\">&lt;div&gt;</code>, with no other element in between</p>\n",
    "</td>\n",
    "</tr>\n",
    "<tr>\n",
    "\n",
    "</tr>\n",
    "</tbody>\n",
    "</table>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[<span class=\"mw-headline\" id=\"Early_life_and_education\">Early life and education</span>,\n",
       " <span class=\"mw-headline\" id=\"Career\">Career</span>,\n",
       " <span class=\"mw-headline\" id=\"World_War_II\">World War II</span>,\n",
       " <span class=\"mw-headline\" id=\"UNIVAC\">UNIVAC</span>,\n",
       " <span class=\"mw-headline\" id=\"COBOL\">COBOL</span>,\n",
       " <span class=\"mw-headline\" id=\"Standards\">Standards</span>,\n",
       " <span class=\"mw-headline\" id=\"Retirement\">Retirement</span>,\n",
       " <span class=\"mw-headline\" id=\"Post-retirement\">Post-retirement</span>,\n",
       " <span class=\"mw-headline\" id=\"Anecdotes\">Anecdotes</span>,\n",
       " <span class=\"mw-headline\" id=\"Death\">Death</span>,\n",
       " <span class=\"mw-headline\" id=\"Dates_of_rank\">Dates of rank</span>,\n",
       " <span class=\"mw-headline\" id=\"Awards_and_honors\">Awards and honors</span>,\n",
       " <span class=\"mw-headline\" id=\"Military_awards\">Military awards</span>,\n",
       " <span class=\"mw-headline\" id=\"Other_awards\">Other awards</span>,\n",
       " <span class=\"mw-headline\" id=\"Legacy\">Legacy</span>,\n",
       " <span class=\"mw-headline\" id=\"Places\">Places</span>,\n",
       " <span class=\"mw-headline\" id=\"Programs\">Programs</span>,\n",
       " <span class=\"mw-headline\" id=\"In_popular_culture\">In popular culture</span>,\n",
       " <span class=\"mw-headline\" id=\"Grace_Hopper_Celebration_of_Women_in_Computing\">Grace Hopper Celebration of Women in Computing</span>,\n",
       " <span class=\"mw-headline\" id=\"Notes\">Notes</span>,\n",
       " <span class=\"mw-headline\" id=\"Obituary_notices\">Obituary notices</span>,\n",
       " <span class=\"mw-headline\" id=\"See_also\">See also</span>,\n",
       " <span class=\"mw-headline\" id=\"References\">References</span>,\n",
       " <span class=\"mw-headline\" id=\"Further_reading\">Further reading</span>,\n",
       " <span class=\"mw-headline\" id=\"External_links\">External links</span>]"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# note depending on your IP Address, \n",
    "# this class may be called something different\n",
    "soup.select(\".toctext\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Early life and education\n",
      "Career\n",
      "World War II\n",
      "UNIVAC\n",
      "COBOL\n",
      "Standards\n",
      "Retirement\n",
      "Post-retirement\n",
      "Anecdotes\n",
      "Death\n",
      "Dates of rank\n",
      "Awards and honors\n",
      "Military awards\n",
      "Other awards\n",
      "Legacy\n",
      "Places\n",
      "Programs\n",
      "In popular culture\n",
      "Grace Hopper Celebration of Women in Computing\n",
      "Notes\n",
      "Obituary notices\n",
      "See also\n",
      "References\n",
      "Further reading\n",
      "External links\n"
     ]
    }
   ],
   "source": [
    "for item in soup.select(\".toctext\"):\n",
    "    print(item.text)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Example Task 3 - Getting an Image from a Website\n",
    "\n",
    "Let's attempt to grab the image of the Deep Blue Computer from this wikipedia article: https://en.wikipedia.org/wiki/Deep_Blue_(chess_computer)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "res = requests.get(\"https://en.wikipedia.org/wiki/Deep_Blue_(chess_computer)\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "soup = bs4.BeautifulSoup(res.text,'lxml')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "image_info = soup.select('.thumbimage')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[<img alt=\"\" class=\"thumbimage\" data-file-height=\"601\" data-file-width=\"400\" decoding=\"async\" height=\"331\" src=\"//upload.wikimedia.org/wikipedia/commons/thumb/b/be/Deep_Blue.jpg/220px-Deep_Blue.jpg\" srcset=\"//upload.wikimedia.org/wikipedia/commons/thumb/b/be/Deep_Blue.jpg/330px-Deep_Blue.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/b/be/Deep_Blue.jpg 2x\" width=\"220\"/>,\n",
       " <img alt=\"\" class=\"thumbimage\" data-file-height=\"600\" data-file-width=\"800\" decoding=\"async\" height=\"165\" src=\"//upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Kasparov_Magath_1985_Hamburg-2.png/220px-Kasparov_Magath_1985_Hamburg-2.png\" srcset=\"//upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Kasparov_Magath_1985_Hamburg-2.png/330px-Kasparov_Magath_1985_Hamburg-2.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Kasparov_Magath_1985_Hamburg-2.png/440px-Kasparov_Magath_1985_Hamburg-2.png 2x\" width=\"220\"/>]"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "image_info"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(image_info)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "computer = image_info[0]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "bs4.element.Tag"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
 
