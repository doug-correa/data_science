Introduction

Recently, I have been working on an open-source application aimed at empowering individuals to unlock their full potential, stay organized, and achieve their goals effectively by leveraging the power of smart algorithms and AI. One of the features of this application is a virtual library system where users can catalog and rate books they have recently read and those they are interested in reading in the future. They can also search for new books in a comprehensive database and receive personalized recommendations based on their interests and ratings. One of the recommendation algorithms used in this project is a collaborative filtering system, which will be presented here:
You can access the code I used in this project in this [notebook]().

Dataset

The dataset used is a publicly available dataset found on Kaggle since, being a project in development, there are currently no users generating data to feed the collaborative filtering system. This dataset can be accessed [here]().

Exploratory Data Analysis and Data Transformation

1.There were 2 rows with missing data in the dataset, and they were removed.
2. To make the calculation manageable for computer memory, I filtered only books that have more than 50 ratings.

Algorithm

1. I inputted the data into a matrix.
2. I calculated the matrix using Pearson correlation.
3. I ranked the top 10 users most similar to the target user.
4. I ranked the top 10 books with the highest ratings among the books read by similar users to the target user that have not been read by the target user yet.



