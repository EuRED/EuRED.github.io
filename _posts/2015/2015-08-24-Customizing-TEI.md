---
title: 'Customizing TEI'
date: 2015-08-24T05:12:40.000Z
last_modified_at: 2015-08-24T05:43:57.000Z
featured_image: /uploads/2015/20150824-rosetta.jpg
---

# Customizing TEI

Here it comes, step by step processes to customize TEI.

## Add an element

1. Go to [www.tei-c.org/Roma](http://www.tei-c.org/Roma)
2. Upload a customization : ODD file (One Document Does it all)
3. Click Start
4. Click on **Add elements** tab
5. Add all the elements you want to add

## Add an attribute

1. Go to [www.tei-c.org/Roma](http://www.tei-c.org/Roma)
2. Upload a customization : ODD file (One Document Does it all)
3. Click Start
4. Click on **Add elements** tab
5. To the right of your newly added element, you will find **Change attributes**
6. Either click on **Add new attributes** or on the **name** of your newly added attribute if any
7. Choose if it is **optional**
8. **Contents** : which type, which quantity
9. **Defaut value** if any
10. **Closed list** and **list of values** if required
11. Add some **description**
12. **Save**
13. **Save** (twice)

## What is really lackin here

Element content is at the bottom of the page, it should be written in valid XML. But there's no assistant for that here.

## Where to find some documentation

[http://www.tei-c.org/Guidelines/Customization/odds.xml](http://www.tei-c.org/Guidelines/Customization/odds.xml)

## How to write directly inside the ODD

Some of the treatments cannot be easily defined inside ROMA, that's where a text editor is welcome.

If you open your ODD inside a text editor, have a look at the declaration of a new attribute :

````xml
<elementSpec ident="readerStatus" ns="http://readingexp.univ-lemans.fr/tei" mode="add">
    <desc/>
    <classes>
        <memberOf key="att.global"/>
    </classes>
    <content>
        <rng:zeroOrMore>
            <rng:choice>
                <rng:attribute>
                    <rng:anyName/>
                </rng:attribute>
                <rng:text/>
                <rng:element>
                    <rng:anyName/>
                    <ref name="readerStatus"/>
                </rng:element>
            </rng:choice>
        </rng:zeroOrMore>
    </content>
    <attList>
        <attDef ident="ref" mode="add">
            <desc>Reference</desc>
            <datatype minOccurs="0" maxOccurs="1">
                <rng:ref name="data.text"/>
            </datatype>
        </attDef>
    </attList>
</elementSpec>
````

This readerStatus element has its classes, contents and attributes defined here, and can contain itself (look <ref name="readerStatus"/> inside the content).
