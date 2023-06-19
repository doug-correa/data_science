### Introduction

Recently, I have been working on an open-source application aimed at empowering individuals to unlock their full potential, stay organized, and achieve their goals effectively by leveraging the power of smart algorithms and AI. One of the features of this application is a virtual library system where users can catalog and rate books they have recently read and those they are interested in reading in the future. They can also search for new books in a comprehensive database and receive personalized recommendations based on their interests and ratings. One of the recommendation algorithms used in this project is a collaborative filtering system, which will be presented here:

You can access the code I used in this project in this [notebook](https://github.com/dougpcorrea/data_science/blob/main/2.%20Books%20recomendation%20collaborative%20filtering/book_recomendation.ipynb).

### Dataset

The dataset used is a publicly available dataset found on Kaggle since, being a project in development, there are currently no users generating data to feed the collaborative filtering system. 

This dataset can be accessed [here]().

### How the algorithm was built

The data was inputted into a matrix and calculated using Pearson's correlation. The recommended books are the top x books with the highest average rating among y users who have the highest degree of similarity with the target user, excluding books already read by the target user.

### How it was implemented on the system

Each time the user rates a new book, an asynchronous task is called on the back-end, and the user matrix is then calculated. Once the calculation is complete, the recommended books for the user are updated in the database. As soon as the user accesses the recommendations page again, they will see the possible new recommendations discovered by the algorithm.



