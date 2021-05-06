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
This Python script enables the Colorado Board of Elections to quickly analyze the compiled election data to discover which candidate won the recent local congressional election. The script also shows the county with the highest voter turnout. We propose that the Board will be able to use this Python script to analyze future elections as well, albeit with a few simple modifications. For example, say the Board was interested in the turnout for each Colorado city in which the election was held. We replaced some of the county names in the "election_results.csv" data file with cities that are located in those counties. Jefferson County has been split into the cities of Golden, Kittredge, and Pine. Arapahoe County has been split into the cities of Centennial, Englewood, and Glendale. All of the candidate votes for those counties remained the same. The following code block shows the changes that were made to the script to perform a city specific analysis:

- All "county" variables and strings have been changed into "city" variables and strings. This isn't necessary, but shows that the variables and strings can be changed to match any new data

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

Another future purpose for this Python script could be to add an analysis of additional ballot questions. For example, say the Board of Elections was interested in the outcomes for a hypothetical district proposition, called Proposition A. An additional column of data was added to the "election_results.csv" data file that shows possible voter choices for Proposition A: "Yay" or "Nay". Since we are assuming every voter would have answered the Proposition A ballot question, the total number of "Yay" or "Nay" votes is equal to the previously counted vote total (369,711). The following code blocks shows how our script can be modified to add additional analyses:

- We can establish new lists and dictionaries to hold the values for our Prop A analysis

```python
# Prop A Options and proposition votes.
prop_options = []
prop_votes = {}
```

- We can create new variables to track the Prop A outcome, vote count, and percentages

```python
# Track the proposition outcome, vote count, and percentage
prop_outcome = ""
prop_count = 0
prop_percentage = 0 
```

- We can replicate our previous "if-then" statements to track all Prop A choices and "Yay"/"Nay" votes

```python
# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the city name from each row.
        city_name = row[1]

        # Get the proposition choice from each row.
        prop_name = row[3]
        
        . . . . . . . . . . . . . . . . . . . . . .
        
        # If the proposition choice does not match any existing choice add it to
        # the proposition list
        if prop_name not in prop_options:

            # Add the candidate name to the candidate list.
            prop_options.append(prop_name)

            # And begin tracking that candidate's voter count.
            prop_votes[prop_name] = 0

        # Add a vote to that candidate's count
        prop_votes[prop_name] += 1
```
        
- Finally, we can replicate our previous "for" loop that prints all "Yay" and "Nay" votes and then calculates the winning Proposition A outcome

```python
# Save the final proposition vote count to the text file.
    for prop_name in prop_votes:

        # Retrieve vote count and percentage
        votes = prop_votes.get(prop_name)
        prop_percent = float(votes) / float(total_votes) * 100
        prop_results = (
            f"{prop_name}: {prop_percent:.1f}% ({votes:,})\n")

        # Print each prop's vote count and percentage to the
        # terminal.
        print(prop_results)
        #  Save the prop results to our text file.
        txt_file.write(prop_results)

        # Determine winning vote count, winning percentage, and proposition outcome.
        if (votes > prop_count) and (prop_percent > prop_percentage):
            prop_count = votes
            prop_outcome = prop_name
            prop_percentage = prop_percent

    # Print the winning candidate (to terminal)
    winning_prop_summary = (
        f"-------------------------\n"
        f"Proposition A Outcome: {prop_outcome}\n"
        f"Winning Prop Count: {prop_count:,}\n"
        f"Winning Prop Percentage: {prop_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_prop_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_prop_summary)
```

- The "election_analysis.txt" file now shows our results with the new Proposition A data added to the end of the document. Our hypothetical Proposition A won in a landslide!

![Image of Proposition A Analysis](https://github.com/jpb12002/Election_Analysis/blob/main/Proposition_Analysis.png)

