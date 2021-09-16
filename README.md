# Mission-to-Mars:  Just Scraping the Surface 
##  Resources: 
Data Source: Mission_to_Mars.ipynb, app.py, scraping.py and index.html

Data Tools: Jupyter Notebook, Python and MongoDB

Software: MongoDB, Python 3.8.3, Visual Studio Code 1.50.0, Flask Version 1.0.2

<p align="center">
   <img width="600" height="300" src="https://github.com/jacquie0583/Mission-to-Mars/blob/main/image%205.png">
</p>  

##  Goal
Use BeautifulSoup, Splinter, and Pandas to scrape five different webpages related to Mars, and display the results on a webpage using MongoDB and Flask.
##  Process
###  Scraping Mars Data
Scraping began in Jupyter notebook to test the code. After importing the necessary dependencies,  the chromedriver was installed to facilitate my browser to open each webpage that needed scraping. Scraping began with the NASA Mars News website for the title and text of the most recent article, storing the results in variables to be referenced later. BeautifulSoup was utilized to parse through the HTML and search for the appropriate elements and classes that contained the information I necessary for soup.find_all(). Because the results come back as a list, indexing the first item proved necessary and obtained the text from there.


<p align="center">
   <img width="500" height="200" src="https://github.com/jacquie0583/Mission-to-Mars/blob/main/image%20%201.png">
</p>  

Moving to the Jet Propulsion Laboratory’s to capture an image of the Dusty Space Cloud; acquiring a full-sized featured image. Parsing the resulting html with soup, adding try/except for error handling and using the base URL to create an absolute URL assisted in accomplishing my goal. Afterwards, I used soup.find_all() and .a[‘href’] to find the relative image path, which I combined with the main URL to get the full-sized image.


<p align="center">
   <img width="600" height="400" src="https://github.com/jacquie0583/Mission-to-Mars/blob/main/image%204.png">
</p>  


To accompany the pictures scraping Mars facts from the galaxyfacts-mars website will enhance the project. Because the data was stored in a table, Pandas provided a means of scraping instead of BeautifulSoup.   The command, pd.read_html, was used to scrape the table.  Anticipating possible issues, a try/except for error handling was added.   I then renamed the columns and set the index before converting that data frame into an HTML table with df.to_html().

<p align="center">
   <img width="500" height="500" src="https://github.com/jacquie0583/Mission-to-Mars/blob/main/image%206.png">
</p>  


Lastly, acquiring the images and names of all four of Mars’ hemispheres from the USGS Astrogeology page became the next mission.  Searching for the hemisphere titles with BeautifulSoup and storing the names in a list, hemisphere_image_urls.  Filtering through the hemispheres, thumbnail links were gathered and iterated through the results with a conditional, checking if the thumbnail element contained an image. If true, the relative image path was taken and combined with the main URL, and the full image URL was appended to an empty list (thumbnail_links) outside of the loop. To obtain the full-sized images of the hemispheres, I then iterated through each link in thumbnail_links, searching for all img elements with a wide-image class. The results were used to retrieve the full image path of the hemispheres, with the URLs being stored in a list, hem_dict. To match the hemisphere name to the correct image link, I zipped together hemisphere_image_urls
and hem_dict  and iterated through that zipped object, first appending the hemisphere title to an empty dictionary as a key, and the image URL as the value, and then appending that dictionary to an empty list.
After all the code was checked, it was then transferred from the notebook to a Python file and used to create a scraping function. The results of all the scraping was then stored as a dictionary to be returned at the end of the function.

<p align="center">
   <img width="800" height="600" src="https://github.com/jacquie0583/Mission-to-Mars/blob/main/image%207.png">
</p>  

###  Flask
In a separate file, Flask was used to trigger the scrape function, update the Mongo database with the results, and then return that record of data from the database on a webpage.
An instance of Flask was created, and PyMongo was used to establish a connection to the MongoDB server. With this connection, the /scrape route was used to run the scrape function located in the imported scraping.py file. Updating the Mongo database with the new collection from the scrape, was accomplished using update and upsert=True. The end of this route redirects to the home route. The home route searches for one record of data in the Mongo database and then renders the index.html template with that record.
In index.html, the /scrape route was linked to a button, which a user could click to initiate the scrape. The remainder of that HTML file was formatted with Bootstrap to display the results from the scrape.

##  Step 1 - Scraping
### NASA Mars News
   •	Scraped the NASA Mars News Site and collected the latest News Title and Paragraph Text
    
### JPL Mars Space Images - Featured Image
   •	Visited the URL for the JPL Featured Space Image
   
   •	Use Splinter to navigated the site and found the image URL for the current Featured Mars Image and assigned the URL string to a variable called img_url
   
   •	Made sure to find the image URL to the full size .jpg image
   
   •	Made sure to save a complete URL string for this image
### Mars Facts
   •	Visited the Mars Facts webpage and used Pandas to scrape the table containing facts about the planet including Diameter, Mass, etc.

   •	Use Pandas to convert the data to a HTML table string

### Mars Hemispheres
   •	Visited the USGS Astrogeology site to obtain high resolution images for each of Mar's hemispheres 
   
   •	Saved both the image URL string for the full resolution hemisphere image, and the Hemisphere title containing the hemisphere name.
   
    o Used a Python dictionary to store the data using the keys hemisphere_image and title
       
   •	Appended the dictionary with the image URL string and the hemisphere title to a list
   
    o This list contains one dictionary for each hemisphere
      
## Step 2 - MongoDB and Flask Application
   •	Used MongoDB with Flask template to create a new HTML page that display all of the information that was scraped from the URLs above
   
   •	Converted Jupyter Notebook into a Python Script called scraping.py with a function called scrape that will executed all of the scraping code from above and return one Python       Dictionary containing all of the scraped data
   
   •	Created a route called /scrape that imported the scraping.py script and called the scrape function
   
     o Store the return value in Mongo as a Python Dictionary
    
   •	Created a root route / that queried the Mongo database and passed the Mars Data into an HTML template to display the data
   
   •	Created a template HTML file called index.html that took the Mars Data Dictionary and displayed all of the data in the appropriate HTML elements
