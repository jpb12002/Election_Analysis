# PyPoll with Python

## Overview of Election Audit
A Colorado Board of Elections employee has given us the the following tasks to complete the election audit of a recent local congressional election.

1. Calculate the total number of votes cast. 
2. Get a complete list of candidates who received votes as well as counties that are being audited.
3. Calculate the total number of votes each candidate and county received.
4. Calculate the percentage of votes each candidate won and the percentage of the votes cast in each county.
5. Determine the largest county turnout and the winner of the election based on the popular vote.

## Resources
- Data Source: election_results.csv
- Software: Python 3.9.4, Visual Studio Code 1.55.2

## Election Audit Results
- There were 369,711 total votes cast in the Colorado local congressional election.
- The vote breakdown for each county is as follows:
  - Arapahoe County: 24,801 votes (6.7%)
  - Denver County: 306,055 votes (82.8%)
  - Jefferson County: 38,855 votes (10.5%)
- Denver County had the largest number of votes, with ~83% of the total votes cast. 
- The vote breakdown for each candidate is as follows:
  - Charles Casper Stockham: 85,213 votes (23.0%)
  - Diana DeGette: 272,892 votes (73.8%)
  - Raymon Anthony Doane: 11,606 votes (3.1%)
- Diana DeGette won the election with 272,892 votes and ~74% of the total votes cast. 

## Election Audit Summary
This Python script enables the Colorado Board of Elections to quickly analyze the compiled election data to discover which candidate won the recent local congressional election. The script also shows the county with the highest voter turnout. We propose that the Board will be able to use this Python script to analyze future elections as well, albeit with a few simple modifications. For example, say the Board was interested in the turnout for each Colorado city in which the election was held. We replaced some of the county names in the "election_results.csv" data file with cities that are located in those counties. Jefferson County has been split into the cities of Golden, Kittredge, and Pine. Arapahoe County has been split into the cities of Centennial, Englewood, and Glendale. All of the candidate votes for those counties remained the same. The following images show the changes to the code script that now show a city specific analysis:

- All "county" variables and strings have been changed into "city" variables and strings. This isn't necessary, but shows that the variables and strings can be changed to match any new data:

```python
 # 6a: Write a for loop to get the city from the city dictionary.
    for city_name in city_votes:
        # 6b: Retrieve the city vote count.
        votes = city_votes.get(city_name)
        # 6c: Calculate the percentage of votes for the city.
        vote_percentage = float(votes) / float(total_votes) * 100
         # 6d: Print the city results to the terminal.
        city_results = (
            f"{city_name}: {vote_percentage:.1f}% ({votes:,})\n")
        print(city_results)
         # 6e: Save the city votes to a text file.
        txt_file.write(city_results)
         # 6f: Write an if statement to determine the winning city and get its vote count.
        if (votes > largest_city_turnout):
            largest_city_turnout = votes
            largest_city_name = city_name

    # 7: Print the city with the largest turnout to the terminal.
    winning_city_summary = (
        f"\n-------------------------\n"
        f"Largest City Turnout: {largest_city_name}\n"
        f"-------------------------\n")
    print(winning_city_summary)

    # 8: Save the city with the largest turnout to a text file.
    txt_file.write(winning_city_summary)
```
- Now the "election_analysis.txt" file shows our results with the new city data in place of the county data. The same changes can be made for new candidates that run in future elections. 

![Image of City Election Analysis](https://github.com/jpb12002/Election_Analysis/blob/main/City_Analysis.png)

