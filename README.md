#Python-Goose - Article Extractor

##Intro


Goose was originally an article extractor written in Java that has most recently (aug2011) converted to a scala project by Gravity.com

This is a complete rewrite in python. The aim of the software is is to take any news article or article type web page and not only extract what is the main body of the article but also all meta data and most probable image candidate.

Goose will try to extract the following information:

 - Main text of an article
 - Main image of article
 - Any Youtube/Vimeo movies embedded in article (TODO)
 - Meta Description
 - Meta tags


Originally, Goose was open sourced by Gravity.com in 2011

 - Lead Programmer: Jim Plush (Gravity.com)
 - Contributers: Robbie Coleman (Gravity.com)

The python version was rewrite by:

 - Xavier Grangier (Recrutae.com)

##Licensing
If you find Goose useful or have issues please drop me a line, I'd love to hear how you're using it or what features should be improved

Goose is licensed by Gravity.com under the Apache 2.0 license, see the LICENSE file for more details

##Setup
    mkvirtualenv --no-site-packages goose
    git clone https://github.com/xgdlm/python-goose.git
    cd python-goose
    pip install -r requirements.txt
    python setup.py install
    
    
    

##Take it for a spin
    >>> from goose.Goose import Goose
    >>> url = 'http://edition.cnn.com/2012/02/22/world/europe/uk-occupy-london/index.html?hpt=ieu_c2'
    >>> g = Goose()
    >>> article = g.extractContent(url=url)
    >>> article.title
    u'Occupy London loses eviction fight'
    >>> article.metaDescription
    "Occupy London protesters who have been camped outside the landmark St. Paul's Cathedral for the past four months lost their court bid to avoid eviction Wednesday in a decision made by London's Court of Appeal."
    >>> article.cleanedArticleText[:150]
    (CNN) -- Occupy London protesters who have been camped outside the landmark St. Paul's Cathedral for the past four months lost their court bid to avoi
    >>> article.topImage.imageSrc
    http://i2.cdn.turner.com/cnn/dam/assets/111017024308-occupy-london-st-paul-s-cathedral-story-top.jpg


##Goose is now language aware
For exemple scrapping a spanish content page with correct meta language tags

    >>> from goose.Goose import Goose
    >>> url = 'http://sociedad.elpais.com/sociedad/2012/10/27/actualidad/1351332873_157836.html'
    >>> g = Goose()
    >>> article = g.extractContent(url=url)
    >>> article.title
    u'Las listas de espera se agravan'
    >>> article.cleanedArticleText[:150]
    u'Los recortes pasan factura a los pacientes. De diciembre de 2010 a junio de 2012 las listas de espera para operarse aumentaron un 125%. Hay m\xe1s ciudad'

Some pages don't have correct meta language tags, you can force it using configuration :

    >>> from goose.Goose import Goose
    >>> url = 'http://www.elmundo.es/elmundo/2012/10/28/espana/1351388909.html'
    >>> g = Goose({'useMetaLanguge': False, 'targetLanguage':'es'})
    >>> article = g.extractContent(url=url)
    >>> article.cleanedArticleText[:150]
    u'Importante golpe a la banda terrorista ETA en Francia. La Guardia Civil ha detenido en un hotel de Macon, a 70 kil\xf3metros de Lyon, a Izaskun Lesaka y '

Passing 
    {'useMetaLanguge': False, 'targetLanguage':'es'}
will force as configuration will force the spanish language

##Configuration
There is two way to pass configuration to goose. The first one is to pass to goose a Configuration() object. The second one is to pass a configuration dict

For instance, if you want to change the userAgent used by Goose juste pass :

    >>> g = Goose({'browserUserAgent': 'Mozilla'})


##TODO
  - Camel Case method and variables : We have used camelcase code to reflect the orginal scala code.
  - Move modules around.
    This should be changed to a more pythonic naming.
  - Video extraction
  - Fetch faveicon 

##Known issues
  - There is some issue with unicode URLs.

##OS X 10.7 Install Instructions

Installation Help:

1. Install python-devel if you don't have it
2. Install libjpeg
        brew install libjpeg

3. You need to install the python imaging library.  We wont be using it, but its a dependency deep in the goose egg (fun!).

  a. download

        curl -O -L http://effbot.org/downloads/Imaging-1.1.7.tar.gz

  b. extract

        tar -xzf Imaging-1.1.7.tar.gz
        cd Imaging-1.1.7

  c. build and install

        python setup.py build
        sudo python setup.py install

4. Next up clone this repo and install the egg.

5. Once you install the egg you have to then copy the resources directory manually into the egg.  There is something screwy about the way its setup.

6. Create and chown the Goose temp directory "/tmp/goosetmp"