Hello and welcome to Uchenna's Fast.ai blog. Edit the `index.md` file to change this content. All pages on the blog, including this one, use [Markdown](https://guides.github.com/features/mastering-markdown/). You can include images:

![Image of fast.ai logo](images/logo.png)

## Scraping Marketing Campaign Images from Bing


The objective of this project was to get as many Docker Campaign Images as possible and to classify them using various labels. 

# "Holiday" - for Campaign pictures run during holidays

# "Dockers Campaign" - For Campaign pictures that had the dockers logo on it

# "Dockers" - This keyword clearly wasn't very descriptive as it picked lots of images of ships in docks.

## Code


# Install Fastbook 
This requires access to your google drive

!pip install -Uqq fastbook

import fastbook

fastbook.setup_book()

#Import Fastbook Libraries

from fastbook import *

from fastai.vision.widgets import *

## To download images with Bing Image Search, You have 2 options:

# 1. Sign up at Microsoft Azure for a free account. 
You will be given a key, which you can copy and enter in a cell as follows (replacing 'XXX' with your key and executing it):

#key = os.environ.get('AZURE_SEARCH_KEY', 'your API Key')

Once you've set key, you can use search_images_bing function.

#search_images_bing

#results = search_images_bing(key, 'grizzly bear')

#ims = results.attrgot('content_url')

#len(ims)

# 2. You can install the Bing Image Downloader. 
I chose this option because it was a lot easier to deal with than searching for API keys. Note that the setback is that the data may not be as recent as a live search - but since the data i required for this project was mostly historical - this was not an issue.

pip install bing-image-downloader
from bing_image_downloader import downloader 

# forced limit to 150 images 

for q in ["dockers campaign", "dockers", "holiday"]: 

  downloader.download(q, limit=150, output_dir='dockers', adult_filter_off=True, force_replace=False, timeout=5)


# To view an Image from the results, copy a link from the search results above

ims = ['https://howtobearedhead.com/wp-content/uploads/2014/12/how_to_be_A-redhead_kids_christmas.jpg']

#Note that this file has to be renamed if you change the link

dest = 'image.jpg'

download_url(ims[0], dest)

im = Image.open(dest)

im.to_thumb(128,128)

# Downloading all the URLs for each search term:

Image_types = "dockers campaign", "dockers", "holiday"

path = Path('dockers')

if not path.exists():

    path.mkdir()
    
    for o in Image_types:
    
        dest = (path/o)
        
        dest.mkdir(exist_ok=True)
        
        results = search_images_bing(key, f'{o} dockers')
        
        download_images(dest, urls=results.attrgot('contentUrl'))
        
        
# Chaecking if our folder has images in it:

fns = get_image_files(path)

fns

# Checking to see if any of the files are corrupt:

failed = verify_images(fns)

failed

If any of the files had beemn corrupt , we would have to remove them. To remove all the failed images, you can use unlink on each of them. Note that, like most fastai functions that return a collection, verify_images returns an object of type L, which includes the map method. This calls the passed function on each element of the collection:

failed.map(Path.unlink);

...and that's all she wrote.

## References

Code resource is from Jeremy Howard's Fast.ai Course -[link to fast.ai](https://www.fast.ai) - Deep Learning for Coders with Fastai and Pytorch.

Note that you can clean the images further using the codes in his notebook. Since the intention of this project was to scrape images and classify them, my job here is done. 

Happy Learning.

