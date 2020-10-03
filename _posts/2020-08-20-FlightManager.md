---
title: "Concurrent Flight Manager"
date: 2020-8-20
tags: [Database, Java, Class_Project]
header:
excerpt: "Concurrent Flight Manager is a Java terminal interface that allows you to log in as a user and book a number of flights. SQL database hosted on Azure was used."
mathjax: "true"
---
# Concurrent Flight Manager
## Overview:

Concurrent Flight Manager is a Java terminal interface that allows you to log in as a user and book a number of flights. Users can create accounts, where passwords are stored on the database, passwords are hashed and stored with salt for verification. Multiple requests can be made, with transactions handled on a MySQL server hosted on Azure, simultaneous requests and deadlocks are resolved in Java. 

## How I built it:

First a Flight Database was imported on to Azure, in addition to the flight database I also imported two empty schemas for users and flight reservations. When a new user was created it would add the user information onto the database. Once a user was logged on in the Java terminal, they would be able to access and book flights through source city and destination cities. In order to safely allow for multiple users to book flights, a 2 phase locking machenisim with commit and roll back was implemented. When one user access the data base it begins the transaction, only commiting once all changes to that database has been made. If a commit could not be made it would roll back to when the transaction started and try again.

## How to try it out:

Contant me through email to get access to the following code. Access has been restricted since the project is an ongoing homework project for CSE 344 Intro to Data Management at UW Seattle.

## Sources

Ryan Maas. 2020. Flight Manager. University of Washington Assingment. https://sites.google.com/cs.washington.edu/cse-344-20su/home
