# Analysis of Election Audit

## Overview of Election Audit

The client has provided an excel document containing the data of a recent election for a congressional district in Colorado. They have requested a way to audit the data that is automated and applicable not only to this district, but to other districts and levels of elections in the future. 

### Purpose
This python code will automate the classification and calculation of the data contained in the excel document in order to provide the total votes and percentage of votes of each county and candidate, as well as the winning candidate and the county with the most turnout, in a readable manner. Election officials will be able to apply this to datasets pertaining to different elections and districts in order to calculate the results of those elections as well. 

## Election Audit Results

- Total Votes
    - In order to calculate the total number of votes I assigned the variable **total_votes** to equal 0 to start with. Then, in a for loop, I added 1 to **total_votes** each time the loop went to the next row, or vote, in the dataset. 
        ```
        for row in reader:
            total_votes = total_votes + 1
        ```
    -  This resulted in the total votes of the election, which was 369,711. This was written onto a text file by initializing a variable to equal an f string that, when written onto a text file, would appear as this:
        ```
        Election Results
        -------------------------
        Total Votes: 369,711
        -------------------------
        ```

- County Breakdown
    - Total Votes
        -  For the total of each county's votes I first created a list called **county_options** that would contain a list of the counties involved in the election, and a dictionary called **county_votes** that would store the counties as keys and the number of votes they had as values. Then, within the same for loop that was used to calculate the total votes, I assigned the variable **county_name** to be the name of the county that appeared in that row, or vote. I then created an If statement that checked whether the **county_name** was included in **county_options** yet, and if not, then it would be added to the **county_options** list and the value assigned to the **county_name** key in the **county_votes** dictionary would be set to 0. After that, outside of the If statement, 1 would be added to the value assigned to that **county_name**'s key. Since this happened outside of the If statement, every time the for loop went to the next row 1 would be added to the value in the dictionary if the **county_name** already existed in the **county_options** list.
            ```
            county_options = []
            county_votes = {}

            for row in reader:

                county_name = row[1]

                if county_name not in county_options:

                    county_options.append(county_name)

                    county_votes[county_name] = 0

                county_votes[county_name] += 1
            ```

    - Percentage of Votes
        - In order to calculate the percentage each county had of the vote, I created a new for loop that would go through every county in the **county_options** dictionary. I then created a variable that would contain the amount of the votes that the county had called **county** and a variable that would be the result of the amount of votes that county had divided by the total votes of the elections multiplied by 100 called **county_percentage**. 
       
            ```
            for county_name in county_options:
                
                county = county_votes[county_name] 
                
                county_percentage = float(county) / float(total_votes) * 100
            ```
        - Once I had the **county_percentage** I was able to create a variable that contained it and **county** within an f string that, when written to a text file, would output the percentage each county had of the votes as well as the total votes of the county. 
            ```
            county_results = (f"{county_name}: {county_percentage:.1f}% ({county:,})\n")
            print(county_results)
            
            txt_file.write(county_results)
            ```

    - Results
        - The county with the greatest turnout was Denver, with 306,055 votes and 82.8% of the vote. In second was Jefferson, with 38,855 votes and 10.5% of the vote. In last was Arapahoe with 11,606 votes and 6.7% of the vote. This appeared in the text file as this:
            ```
            County Votes:
            Jefferson: 10.5% (38,855)
            Denver: 82.8% (306,055)
            Arapahoe: 6.7% (24,801)
            ```
- Largest Turnout
     - In order to calculate the county that experienced the largest turnout, I inititalized two variables near the beginning of the code, which were **largest_turnout** to equal an empty string and **largest_count** to equal 0. I then added an IF statement at the end of the for loop that calculated the **county_percentage**. For every county the loop cycled through, it checked whether that county's total votes was greater than **largest_count**, and if it was, then it would assign **largest_count** to equal that county's total votes and **largest_turnout** to equal that county's name. Through this logic, after the for loop had completed its loops, the county that had the greatest amount of total votes would be recorded in **largest_count** and **largest_turnout**. The code looked like this:
        ```
        largest_turnout = ""
        largest_count = 0

        ...

        for county_name in county_options:

            ...
            
            if (county > largest_count):
                largest_count = county
                largest_turnout = county_name
        ```
    - A variable called **biggest_county** was then initialized to equal an f string that contained **largest_turnout**, which was then written onto the text file. This returned the county Denver, which had the most voters, likely due to the fact that the county encompassed the city of Denver which has the largest population in the state. The results appeared in the text file as this:
        ```
        -------------------
        Largest County Turnout: Denver
        -------------------
        ```
- Candidate Breakdown
    - Total Votes
        - The process for calculating the total votes of each candidate was extremely similar to the calculation of the county votes. For the total of each candidate's votes I first created a list called **candidate_options** that would contain a list of the candidates involved in the election, and a dictionary called **candidate_votes** that would store the candidates as keys and the number of votes they had as values. Then, within the same for loop that was used to calculate the total votes of the election, I assigned the variable **candidate_name** to be the name of the candidate that appeared in that row, or vote. I then created an If statement that checked whether the **candidate_name** was included in **candidate_options** yet, and if not, then it would be added to the **candidate_options** list and the value assigned to the **candidate_name** key in the **candidate_votes** dictionary would be set to 0. After that, outside of the If statement, 1 would be added to the value assigned to that **county_name**'s key. Since this happened outside of the If statement, every time the for loop went to the next row 1 would be added to the value in the dictionary if the **candidate_name** already existed in the **candidate_options** list.

            ```
            candidate_options = []
            candidate_votes = {}

            ...

            for row in reader:

            ...

            candidate_name = row[2]

            if candidate_name not in candidate_options:

                candidate_options.append(candidate_name)

                candidate_votes[candidate_name] = 0

            candidate_votes[candidate_name] += 1
            ```
    - Percentage of Votes
        - Calculating the percentage of votes for each candidate was also very similar to the county percentages. I created a new for loop that would go through every candidate in the **candidate_options** dictionary. I then created a variable that would contain the amount of the votes that the candidate had called **votes** and a variable that would be the result of the amount of votes that candidate had divided by the total votes of the elections multiplied by 100 called **vote_percentage**.  

            ```
            for candidate_name in candidate_votes:

            votes = candidate_votes.get(candidate_name)
            vote_percentage = float(votes) / float(total_votes) * 100
            ```
        - Once I had the **vote_percentage** and **votes** I was able to create a variable that equaled an f string containing them that, when written into the text file, would list the candidates and their total votes and percentage of the vote. This code looked like this:
            ```
            candidate_results = (f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

            txt_file.write(candidate_results)
            ```
    - Results
        - The candidate with the most votes was Diana DeGette with 272,892 votes and 73.8% of the vote. In second was Charles Caspar Stockham with 85,213 votes and 23.0% of the vote, and Raymon Anthony Doane came in last with 11,606 votes and 3.1% of the vote. This was written in the text file and looked like this:
            ```
            Charles Casper Stockham: 23.0% (85,213)
            Diana DeGette: 73.8% (272,892)
            Raymon Anthony Doane: 3.1% (11,606)
            ```
- Winning Candidate
    - For the winning candidate results, I first created near the beginning of the code three variables: **winning_candidate** which equaled an empty string, **winning_count** which equaled 0, and **winning_percentage** which also equaled 0. Then, in the same for loop that went through each **candidate_name** in **candidate_votes**, I created an IF statement at the end that checked whether the **votes** of the current **candidate_name** was greater than **winning_count** and whether the **vote_percentage** of the current **candidate_name** was greater than the **winning_percentage**. If these both were true, than **winning_count** would be assigned the value of the candidate's **votes**, **winning_candidate** would be assigned the candidate's **candidate_name**, and **winning_percentage** would be assigned the candidate's **vote_percentage**. Through this logic, after the for loop completed its loops, the candidate with the most votes would have their values assigned to the "winning" variables.
        ```
        winning_candidate = ""
        winning_count = 0
        winning_percentage = 0

        ...

        for candidate_name in candidate_votes:

        ...

        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage
        ```
    - A variable called **winning_candidate_summary** was then created that equaled an f string that contained **winning_count**, **winning_candidate**, and **winning_percentage**.

         ```   
        winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")

        txt_file.write(winning_candidate_summary)
        ```
            

        This, when written to the text file, would output the winning candidate which was Diana DeGette, with 272,892 votes and 73.8% of the vote. This appeared in the text file as this:
        ```
        -------------------------
        Winner: Diana DeGette
        Winning Vote Count: 272,892
        Winning Percentage: 73.8%
        -------------------------
        ```

## Election Audit Results
This code can be utilized for different elections across different levels of government with minimal tweaking. With similar elections that take place on the state level, you can simply construct a dataset that is formatted exactly the same as the one used in this example and acquire the same output. If you named the file containing the dataset "election_results.csv" you wouldn't have to change anything at all. If the file name did change, all you would have to do is change the file referenced in the **file_to_load** variable to the name of the new dataset's file.
 
 If you wished to include elections across different states, you could add another column to the data file that included the state of the vote tallied. You could then add a list to the code that would contian the states in the data file, such as **state_options**, and a dictionary that contains that state name as the key and the number of votes that state has as the value, such as **state_votes**. In the for loop that goes through every row, or vote, in the data file containing the votes, you could add a variable, such as **state_name**, that would equal the name of the state. You could then use this variable in an IF statement that checks whether that name is in the **state_options** list, and if not, you could add **state_name** to the **state_options** list and set its key's value to equal zero in **state_votes**. Then, outside of the IF statement, you add 1 to the value of the **state_name** key in **state_votes**. This would add 1 to the number of votes a state has each time that state is encountered in the dataset, which would mean that once the for loop ends you would have the total votes of each state, which would be the turnout that each state encountered. 

Another way to modify the code for different elections is maintenance for datasets that may be formatted differently than the one that is being used for this election. Within the for loop that runs through every row in the dataset are two variables that draw the candidate names and county names out: **candidate_name** and **county_name**. They equal the column within the row that is being analyzed within that loop that contains the candidate name and county name respectively. This looks like this:
```
for row in reader:

    ...

        candidate_name = row[2]

        county_name = row[1]
```
If, in a different dataset, these columns were rearranged, the current code would be retrieving the wrong information for these variables. Luckily, all you have to do to fix this is change the number contained in each variable's **row[ ]** to be the number of the new location of that column, with the first column being 0 and each column after that being how many columns away from the first column it is. For example, a data set that has the candidate name in the first column and the county name in the third column would be formatted like so:
```
for row in reader:

    ...

        candidate_name = row[0]

        county_name = row[2]
```

