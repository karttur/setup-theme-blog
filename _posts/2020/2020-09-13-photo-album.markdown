---
layout: post
title: Photo album
modified: 2020-11-13 10:33
previousurl: null
nexturl: null
categories: "blog"
excerpt: How to create a photo album for the Jekyll theme So Simple with Python
tags:
  - Jekyll
  - photo album
  - So Simple Theme
image: avg-rntwi_RNTWI_java_2001-2016_AS
figure1: github-account_karttur_01_menu-new-repo
date: 2020-11-13 10:33
comments: true
share: true
---
**Contents**

## introduction

This short post introduces a Python script that uses an xml file and converts all images in a pre-defined folder to an album post in the Jekyll theme _So Simple_.

## Prerequisits

The Python script is set up in <span class='app'>Eclipse</span>, as outlined in the post on [Setup Eclipse for PyDev](https://karttur.github.io/setup-ide/setup-ide/install-eclipse/) (in another of my blogs).

## Script

The complete script for generating the _So Simple_ Jekyll photo album:

```
'''
Created on 5 Jul 2020

@author: thomasgumbricht
'''

from collections import defaultdict
from xml.etree import cElementTree as ET
from pprint import pprint
import sys
import os
import subprocess
from _ast import Or

def JekyllYaml(pD):
    print (pD)
    yamlDate = '%(y)s-%(m)s-%(d)s' %{'y':pD['datetime'][0:4], 'm':pD['datetime'][4:6],'d':pD['datetime'][6:8]}
    yamlD = {'layout': pD['layout'], 'title': pD['rolltitle'], 'categories': pD['categories'],
             'except': pD['description'], 'tags': pD['keywords'], 'date': yamlDate,
             'modified': yamlDate, 'comments': 'true', 'share': 'true'}
    yamlL = ['---', 'layout: %s' % pD['layout'], 'title: %s' % pD['rolltitle'],
             'categories: %s' % pD['categories'], 'except: %s' % pD['description'],
             'tags:']
    for item in pD['keywords'].split(','):
        yamlL.append(' - %s' % item)
    yamlL.extend( ['date: %s' % yamlDate, 'modified: %s' % yamlDate,
                    'comments: true', 'share: true', '---' ] )

    for row in yamlL:
        print (row)
    return yamlL

def FigClass(pD, xmlFPN):

    overwrite = True
    if pD['overwrite'] in ['False','false','None','none','0']:
        overwrite = False

    srcFP, rollName = os.path.split(xmlFPN)
    rollName = os.path.splitext(rollName)[0]

    bodyL = []

    if pD['figclass'] in ['1', 'single', 'none', 'None', 'NA', 'na']:
        bodyL.append('<figure>')
    elif pD['figclass'] == 'half':
        bodyL.append("<figure class='half'>")
    elif pD['figclass'] == 'third':
        bodyL.append("<figure class='third'>")

    else:
        sys.exit('unknown figclass')

    # Create target folder
    tarFP = pD['tarfp']

    #if not os.path.exists(tarFP):
    #    os.makedirs(tarFP)

    photoFP = os.path.join(tarFP,'photos')
    #if not os.path.exists(photoFP):
    #    os.makedirs(photoFP)
    rollFP = os.path.join(photoFP,rollName)

    if not os.path.exists(rollFP):
        os.makedirs(rollFP)

    print (srcFP)
    # Get all images of <srctype> in the src folder
    for file in os.listdir(srcFP):
        print ('    ', file)
        if file.endswith(pD['srctype']):

            srcFPN = os.path.join(srcFP, file)
            dstFullFN = dstQlFN = '%s_%s_%s.%s' %(rollName, pD['fullsuffix'], os.path.splitext(file)[0], pD['fullkind'])
            dstFullFPN = dstQlFPN = os.path.join(rollFP,dstFullFN)

            #dstQlFN = '%s_%s_%s.%s' %(rollName, pD['fullsuffix'], os.path.splitext(file)[0], pD['qlkind'])
            #dstQlFPN = os.path.join(rollFP,dstQlFN)

            if not os.path.isfile(dstFullFPN) or overwrite:
                #Here is the conversion
                if int(pD['fullxdims']) and int(pD['fullydims']):
                    resize = '%sx%s' %(pD['fullxdimsl'], pD['fullydims'])
                elif int(pD['fullxdims']):
                    resize = '%sx' %(pD['fullxdims'])
                elif int(pD['fullydims']):
                    resize = 'x%s' %(pD['fullydims'])
                else:
                    resize = False

                if int(pD['fullquality']):
                    if resize:
                        cmdL = ['/usr/local/bin/convert', '-resize', resize, '-quality', pD['fullquality'], srcFPN, dstFullFPN]
                    else:
                        cmdL = ['/usr/local/bin/convert',  '-quality', pD['fullquality'], srcFPN, dstFullFPN]
                else:
                    if resize:
                        cmdL = ['/usr/local/bin/convert', '-resize', resize, srcFPN, dstFullFPN]
                    else:
                        cmdL = ['/usr/local/bin/convert', srcFPN, dstFullFPN]


                subprocess.run(cmdL)

                if pD['fullsuffix'] != pD['qlsuffix']:

                    if int(pD['fullxdims']) != int(pD['qlxdims']) or int(pD['fullydims']) != int(pD['qlydims']) or int(pD['fullquality']) != int(pD['qlquality']) or pD['fullkind'] != pD['qlkind']:

                        dstQlFN = '%s_%s_%s.%s' %(rollName, pD['qlsuffix'], os.path.splitext(file)[0], pD['qlkind'])
                        dstQlFPN = os.path.join(rollFP,dstQlFN)

                        #Here is the conversion
                        if int(pD['qlxdims']) and int(pD['qlydims']):
                            resize = '%sx%s' %(pD['qlxdimsl'], pD['qlydims'])
                        elif int(pD['qlxdims']):
                            resize = '%sx' %(pD['qlxdims'])
                        elif int(pD['qlydims']):
                            resize = 'x%s' %(pD['qlydims'])
                        else:
                            resize = False

                        if int(pD['qlquality']):
                            if resize:
                                cmdL = ['/usr/local/bin/convert', '-resize', resize, '-quality', pD['qlquality'], srcFPN, dstQlFPN]
                            else:
                                cmdL = ['/usr/local/bin/convert',  '-quality', pD['qlquality'], srcFPN, dstQlFPN]
                        else:
                            if resize:
                                cmdL = ['/usr/local/bin/convert', '-resize', resize, srcFPN, dstQlFPN]
                            else:
                                cmdL = ['/usr/local/bin/convert', srcFPN, dstQlFPN]

                        subprocess.run(cmdL)

            bodyL.append('<a href="../../photos/%(fp)s/%(fullfn)s"><img src="../../photos/%(fp)s/%(qlfn)s" alt="image"></a>' %{'fp':rollName, 'fullfn':dstFullFN, 'qlfn':dstQlFN})       

    bodyL.append("<figcaption>%s</figcaption>" %(pD['caption']))
    bodyL.append("</figure>")    
    return bodyL

def WritePost(pD, yamlL, bodyL):
    overwrite = True
    if pD['overwrite'] in ['False','false','None','none','0']:
        overwrite = False

    yamlDate = '%(y)s-%(m)s-%(d)s' %{'y':pD['datetime'][0:4], 'm':pD['datetime'][4:6],'d':pD['datetime'][6:8]}

    year = pD['datetime'][0:4]

    srcFP, rollName = os.path.split(xmlFPN)
    rollName = os.path.splitext(rollName)[0]

    # Create target folder
    tarFP = pD['tarfp']

    yearFP = os.path.join(tarFP,'_posts',year)

    if not os.path.exists(yearFP):
        os.makedirs(yearFP)

    postFN =  '%s-%s.md' %(yamlDate, rollName)

    postFPN = os.path.join(yearFP, postFN)
    print (postFPN)
    f = open(postFPN, 'w')
    for row in yamlL:
        print (row)
        f.write (row)
        f.write ('\n')
    f.write ('\n')
    for row in bodyL:
        f.write ('\n')
        f.write (row)
        f.write ('\n')

if __name__ == '__main__':

    xmlFPN = '/Volumes/jet/Pictures_raw/se/2020/se_20200619_midsommar-julita/se_20200619_midsommar-julita.xml'

    xmlTree = ET.parse(xmlFPN)

    root = xmlTree.getroot()

    paramL = []
    valueL = []

    for child in root:
        #print(child.tag, child.attrib)

        for descript in root.iter(child.tag):
            print(child.tag, descript.text, child.attrib)
            #Convert to dictionary
            if child.tag == 'quicklooks':
                paramL.extend(['qlxdims','qlydims','qlkind','qlquality','qlsuffix'])
                valueL.extend([child.attrib['xdim'],child.attrib['ydim'],child.attrib['kind'],child.attrib['quality'],child.attrib['suffix'] ])
            elif child.tag == 'fullsize':
                paramL.extend(['fullxdims','fullydims','fullkind','fullquality','fullsuffix'])
                valueL.extend([child.attrib['xdim'],child.attrib['ydim'],child.attrib['kind'],child.attrib['quality'],child.attrib['suffix'] ])
            else:
                paramL.append(child.tag)
                valueL.append(descript.text)

    print (paramL)
    print (valueL)

    pD = dict(zip(paramL, valueL))  

    print (pD)

    yamlL = JekyllYaml(pD)

    bodyL = FigClass(pD, xmlFPN)

    WritePost(pD, yamlL, bodyL)
```

## xml

The parameters that define the picture sizes and qualities, layout and caption etc. The xml file must be in the same folder as the images to use for creating the album. Further, the xml file should have the same name as the parent folder. The xml file for the images in the folder _se_20200619_midsommar-julita_:

```
<?xml version='1.0' encoding='utf-8'?>
  <roll>
  	<overwrite>False</overwrite>
    <author>Thomas Gumbricht</author>
    <datetime>20200619</datetime>
    <source>Digital photo</source>
    <camera>Nikon D7000</camera>
    <keywords> Sweden, Julita, Granlund, midsommar, Nelander</keywords>
    <comment>Another pleasant and successful midsommar with the Nelander family at Granlund, Julita, 2020</comment>
    <description>Familjen Nelanders midsommarsfest, Granlund 2020. Thomas och Gabriela Gumbricht fotograferade</description>
    <caption>Familjen Nelanders midsommarsfest, Granlund 2020. Thomas och Gabriela Gumbricht fotograferade.</caption>
    <person></person>
    <country>se</country>
    <place>Julita</place>
    <theme0>midsommar</theme0>
    <theme1>party</theme1>
    <biome>NA</biome>
    <quality>4</quality>
    <public>3</public>
    <paltyp>3</paltyp>
    <track></track>
    <movemode>foot</movemode>
    <media>photo</media>
    <log></log>
    <wp>na</wp>
    <lon>16.068218</lon>
    <lat>59.100210</lat>
    <rolltitle>Midsommarfest, Granlund 2020</rolltitle>
    <figclass>third</figclass>
    <srctype>JPG</srctype>
    <quicklooks xdim='1200' ydim='0' kind='jpg' quality='80' suffix='m'></quicklooks>
    <fullsize xdim='1200' ydim='0' kind='jpg' quality='80' suffix ='m'></fullsize>
    <layout>post</layout>
    <categories>privatephotos</categories>
    <tarfp>/Users/thomasgumbricht/GitHub/private</tarfp>  
  </roll>
```

## Album

The photo album created from the script and xml file above are available as the post [Midsommarfest, Granlund 2020](https://karttur.github.io/private/privatephotos/se_20200619_midsommar-julita/) in my private pages.
