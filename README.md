The HSN Code Validation Agent is built using Google’s conceptual ADK (Agent Developer Kit) framework. Its main goal is to check if the HSN codes provided by the user are correct, based on a master Excel file. The codes can be either a single value or a list. The agent reads this input, looks it up in the Excel file, and gives a clear response.



How the Agent Understands User Input

The agent is designed to recognize when a user wants to validate an HSN code. This is done using the intent called ValidateHSNCode. The actual number entered by the user is captured as an entity called @hsn_code. The input can be typed or spoken, and the agent can handle both one code at a time or many codes at once.



Behind the Scenes: Fulfillment Logic

The main logic happens through a webhook, which is a small service built using Python and Flask. When the agent receives the code from the user, it sends it to this webhook, which checks if the code is valid and prepares a reply.



Loading and Handling the Excel File

To work efficiently, the Excel file (named HSN_Master_Data.xlsx) is loaded only once when the agent starts. It uses the Python pandas library to read the file and stores the data in a dictionary format like this: {HSNCode: Description}. This makes searching for any code very fast.

There is also an option to reload the Excel data using a special command (/reload) in case the file is updated later. This way, there’s no need to restart the whole system.

How Validation Works

The agent checks each HSN code in three steps:
	1.	Format Validation: It checks whether the code is only made up of numbers and is either 2, 4, 6, or 8 digits long.
	2.	Existence Check: It then checks if the exact code exists in the master Excel file.
	3.	(Optional) Hierarchical Check: For longer codes like 8-digit ones, it checks if the shorter “parent” codes like 2-digit or 4-digit versions are also present. This adds more reliability to the validation.


What the Agent Replies
	•	If the code is valid: The agent replies with something like:
“Valid HSN Code: PURE-BRED BREEDING ANIMALS”
	•	If the code is invalid: It gives a reason such as:
“Invalid Code: Not found in master dataset”
or
“Invalid Format: Must be numeric and 2, 4, 6, or 8 digits long”


Development Steps Outline
	1.	Install Python, Flask, Pandas.
	2.	Load Excel file into a dictionary at startup.
	3.	Build Flask API with a POST /validate endpoint.
	4.	Add code validation logic.
	5.	Connect the webhook to the ADK-compatible platform or Dialogflow.
	6.	Test with various inputs.
	7.	 Add reload support for dynamic Excel updates.
