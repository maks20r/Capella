import spacy
import os
import subprocess
from openai import OpenAI
import signal
import time


import datetime
import os.path

from google.auth.transport.requests import Request
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
from googleapiclient.errors import HttpError

nlp = spacy.load("en_core_web_sm")
client = OpenAI(api_key=" ")
SCOPES = ["https://www.googleapis.com/auth/calendar"]


def greet():
    current_time = datetime.datetime.now().time()
    if current_time.hour < 12:
        print("Good morning sir! Ready to change the world?")
    elif current_time.hour < 18:
        print("Good afternoon sir! Let's get to work!")
    else:
        print("Good evening sir! Let's grind!")

#-------------------
#find and opens folders

def find_folder(start_path, target_folder_name):
    for root, dirs, files in os.walk(start_path):
        if target_folder_name in dirs:
            found_path = os.path.join(root, target_folder_name)
            return found_path
    return None

def open_folder(path):
    try:
        subprocess.run(['open', path], check=True)
    except subprocess.CalledProcessError as e:
        print(f"Failed to open folder: {e}")

def search_and_open_folder(target_folder_name):
    start_directory = os.path.join(os.path.expanduser("~"))
    result = find_folder(start_directory, target_folder_name)

    if result:
        print(f"Folder '{target_folder_name}' found at: {result}")
        open_folder(result)
    else:
        print(f"Folder '{target_folder_name}' not found.")

#----------------------
#finds and opens files 

def find_file(start_path, target_file_name):
    for root, dirs, files in os.walk(start_path):
        if target_file_name in files:
            found_path = os.path.join(root, target_file_name)
            return found_path
    return None

def open_file(path):
    try:
        subprocess.run(['open', path], check=True)
        print(f"Opened file: {path}")
    except subprocess.CalledProcessError as e:
        print(f"Failed to open file: {e}")

def search_and_open_file(target_file_name):
    start_directory = os.path.join(os.path.expanduser("~"))
    result = find_file(start_directory, target_file_name)

    if result:
        print(f"File '{target_file_name}' found at: {result}")
        open_file(result)
    else:
        print(f"File '{target_file_name}' not found.")

#-------------------------
#finds and runs python files

#uses  find_file() as well

def run_python_file(path):
    try:
        subprocess.run(['python3', path], check=True)
        print(f"Executed Python file: {path}")
    except subprocess.CalledProcessError as e:
        print(f"Failed to execute Python file: {e}")

def search_and_run_python_file(target_file_name):
    start_directory = os.path.join(os.path.expanduser("~"))
    result = find_file(start_directory, target_file_name)

    if result:
        print(f"Python file '{target_file_name}' found at: {result}")
        run_python_file(result)
    else:
        print(f"Python file '{target_file_name}' not found.")

#--------------------------
#open and close apps

def open_app(app_name):
    try:
        if os.name == "posix":  # Unix-based systems
            subprocess.Popen(['open', '-a', app_name])
            
        else:
            print(f"Unsupported OS: {os.name}")
        print(f"Opening {app_name}...")
    except Exception as e:
        print(f"Error: {e}")

def close_app(app_name):
    try:
        if os.name == "posix":  # Unix-based systems
            subprocess.Popen(['osascript', '-e', f'quit app "{app_name}"'])
        else:
            print(f"Unsupported OS: {os.name}")
        print(f"Closing {app_name}...")
    except Exception as e:
        print(f"Error: {e}")
        
#-------------------------
#create folders

def create_folder(path):
    try:
        os.makedirs(path, exist_ok=True)
        print(f"Folder created: {path}")
    except OSError as e:
        print(f"Error creating folder: {e}")
    
def create_folder_from_input(new_folder_name, parent_folder_name):
    start_directory = os.path.expanduser("~")  
    parent_folder_path = find_folder(start_directory, parent_folder_name)
        
    if parent_folder_path:
        new_folder_path = os.path.join(parent_folder_path, new_folder_name)
        create_folder(new_folder_path)
    else:
        print(f"Parent folder '{parent_folder_name}' not found.")

#-------------------------
#delete folders and files 

def delete_folder(folder_path):
    if os.path.exists(folder_path):
        try:
            subprocess.run(['rm', '-rf', folder_path], check=True)
            print(f"'{folder_path}' deleted successfully.")
        except subprocess.CalledProcessError as e:
            print(f"Failed to delete folder '{folder_path}': {e}")
    else:
        print(f"Folder '{folder_path}' does not exist.")

def delete_folder_from_input(new_folder_name, parent_folder_name):
    start_directory = os.path.expanduser("~")
    parent_folder_path = find_folder(start_directory, parent_folder_name)

    if parent_folder_path:
        new_folder_path = os.path.join(parent_folder_path, new_folder_name)
        delete_folder(new_folder_path)
    else:
        print(f"Parent folder '{parent_folder_name}' not found.")

#-------------------------
#create files

def create_file(file_path):
    if not os.path.exists(file_path):
        try:
            subprocess.run(['touch', file_path], check=True)
            print(f"File '{file_path}' created successfully.")
        except subprocess.CalledProcessError as e:
            print(f"Failed to create file '{file_path}': {e}")
    else:
        print(f"File '{file_path}' already exists.")

def create_file_from_input(new_file_name, parent_folder_name):
    start_directory = os.path.expanduser("~")
    parent_folder_path = find_folder(start_directory, parent_folder_name)

    if parent_folder_path:
        new_file_path = os.path.join(parent_folder_path, new_file_name)
        create_file(new_file_path)
    else:
        print(f"Parent folder '{parent_folder_name}' not found.")
#delete folder function removes files as well 
#delete file_name/folder_name from file_name

#-------------------------
#move folders and files

def move_folder(folder_path, destination_path):
    if os.path.exists(folder_path):
        try:
            subprocess.run(['mv', folder_path, destination_path], check=True)
            print(f"Folder '{folder_path}' moved successfully to '{destination_path}'.")
        except subprocess.CalledProcessError as e:
            print(f"Failed to move folder '{folder_path}': {e}")
    else:
        print(f"Folder '{folder_path}' does not exist.")

def move_folder_from_input(new_folder_name, parent_folder_name, destination_folder_name):
    start_directory = os.path.expanduser("~")
    parent_folder_path = find_folder(start_directory, parent_folder_name)
    destination_folder_path = find_folder(start_directory, destination_folder_name)

    if parent_folder_path and destination_folder_path:
        new_folder_path = os.path.join(parent_folder_path, new_folder_name)
        move_folder(new_folder_path, destination_folder_path)
    else:
        if not parent_folder_path:
            print(f"Parent folder '{parent_folder_name}' not found.")
        if not destination_folder_path:
            print(f"Destination folder '{destination_folder_name}' not found.")
            
#-------------------------
#run top command 

def run_top_command(timeout=5):
    try:
        # Define a function to handle timeout
        def timeout_handler(signum, frame):
            raise TimeoutError("Command timed out")

        # Set a signal alarm for timeout
        signal.signal(signal.SIGALRM, timeout_handler)
        signal.alarm(timeout)  # Set the alarm

        try:
            # Run the 'top' command in batch mode with '-l 5' for 5 seconds
            result = subprocess.run(['top', '-l', '5', '-stats', 'pid,command,cpu,mem'], capture_output=True, text=True)
            
            # Check if the command was successful
            if result.returncode == 0:
                # Split the output into lines and print the first 10 lines
                output_lines = result.stdout.splitlines()
                for line in output_lines[:10]:
                    print(line)
            else:
                print(f"Error: {result.stderr}")

        except Exception as e:
            print(f"An error occurred: {e}")

    except TimeoutError as e:
        print(f"Timeout occurred: {e}")

    finally:
        # Disable the alarm
        signal.alarm(0)

#------------------------------------------------------------------------            
#add even to caledar
            
def book():
    print("Yes of course. Please follow instructions bellow to complete the booking. ")
    creds = None
    # The file token.json stores the user's access and refresh tokens, and is
    # created automatically when the authorization flow completes for the first
    # time.
    if os.path.exists("token.json"):
        creds = Credentials.from_authorized_user_file("token.json", SCOPES)
    # If there are no (valid) credentials available, let the user log in.
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                "credentials.json", SCOPES
            )
            creds = flow.run_local_server(port=0)
        # Save the credentials for the next run
        with open("token.json", "w") as token:
            token.write(creds.to_json())

    try:
        service = build("calendar", "v3", credentials=creds)

        # Call the Calendar API
        now = datetime.datetime.now(datetime.timezone.utc).isoformat()  # Updated to use timezone-aware datetime

        # Get user input for event summary, date, and time
        p_num = input("Please enter you phone number: ")
        summary = input("What procedure are you interested in?: ")
        date_input = input("Enter the date when you would like to come. (MM/DD): ")
        time_input = input("Enter time that would work best for you (HH:MM) in 24-hour format: ")
        print("Thank you for booking an appointment with us. You will recive a confirmation soon.")

        # Parse the date input
        event_date = datetime.datetime.strptime(date_input, "%m/%d")
        # Use current year for the event date
        event_date = event_date.replace(year=datetime.datetime.now().year)

        # Create start and end datetime strings
        start_datetime = event_date.strftime("%Y-%m-%d") + f"T{time_input}:00+04:00"
        end_hour = int(time_input.split(':')[0]) + 1
        end_minute = time_input.split(':')[1]
        end_datetime = event_date.strftime("%Y-%m-%d") + f"T{end_hour:02}:{end_minute}:00+04:00"

        event = {
            "summary": summary,
            "location": "KindCare Clinic",
            "description": p_num,
            "colorId": 1,
            "start": {
                "dateTime": start_datetime,
                "timeZone": "Asia/Dubai"
            },
            "end": {
                "dateTime": end_datetime,
                "timeZone": "Asia/Dubai"
            },
            "recurrence": [
                "RRULE:FREQ=DAILY;COUNT=1"
            ],
            "attendees": [
                #{"email": "social@neuralnine.com"},
                #{"email": "someemailthathopefullydoesnotexist@mail.com"}
            ]
        }

        event = service.events().insert(calendarId='primary', body=event).execute()

        print(f"Event created: {event.get('htmlLink')}")

    except HttpError as error:
        print(f"An error occurred: {error}")


#------------------------------------------------------------------------


def process_message(message, messages):
    doc = nlp(message) 
#open folder 
    if any(token.text == "open" for token in doc) and any(token.text == "folder" for token in doc):
        for i, token in enumerate(doc):
            if token.text == "open":
                target_folder = doc[i + 1].text
                search_and_open_folder(target_folder)
                return f"Opening {target_folder}..."
#open app 
    if any(token.text == "open" for token in doc) and all(token.text != "folder" for token in doc) and all(token.text != "file" for token in doc):    
        for i, token in enumerate(doc):
            if token.text == "open" and token.text !="folder":
                app_name = doc[i + 1].text
                open_app(app_name)
                return f"Opening {app_name}..."
#close app          
    if any(token.text == "close" for token in doc):           
        for i, token in enumerate(doc):
            if token.text == "close":
                app_name = doc[i + 1].text
                close_app(app_name)
                return f"Closing {app_name}..." 
#open file
    if any(token.text == "open" for token in doc) and any(token.text == "file" for token in doc):
        for i, token in enumerate(doc):
            if token.text == "open":
                target_file = doc[i + 1].text
                search_and_open_file(target_file)
                return f"Opening {target_file}..." 
#run file
    if any(token.text == "run" for token in doc) and any(token.text == "file" for token in doc):
        for i, token in enumerate(doc):
            if token.text == "run":
                target_file = doc[i + 1].text
                search_and_run_python_file(target_file)
                return f"Running {target_file}..."   
#create folder
    if any(token.text == "create" for token in doc) and any(token.text == "folder" for token in doc):
        for i, token in enumerate(doc):
            if token.text == "create":
                new_folder_name = doc[i + 2].text
            if token.text == "in":
                parent_folder_name = doc[i + 1].text
                create_folder_from_input(new_folder_name, parent_folder_name)
                return "Creating folder..."
                
        else:
            print("Invalid input format.")
#delete folder and files 
    if any(token.text == "delete" for token in doc):
        new_folder_name = None
        parent_folder_name = None
        for i, token in enumerate(doc):
            if token.text == "delete":
                new_folder_name = doc[i + 1].text
            if token.text == "from":
                parent_folder_name = doc[i + 1].text
                break
        if new_folder_name and parent_folder_name:
            delete_folder_from_input(new_folder_name, parent_folder_name)
            return "Deleting folder..."
        else:
            print("Invalid input format.")
#create file 
    if any(token.text == "create" for token in doc) and any(token.text == "file" for token in doc):
        new_file_name = None
        parent_folder_name = None
        for i, token in enumerate(doc):
            if token.text == "create":
                new_file_name = doc[i + 2].text  # Assuming format: "create file <filename>"
            if token.text == "in":
                parent_folder_name = doc[i + 1].text  # Assuming format: "in <foldername>"
                break
        if new_file_name and parent_folder_name:
            create_file_from_input(new_file_name, parent_folder_name)
            return "Creating file..."
        else:
            print("Invalid input format.")
#move folder and files
    if any(token.text == "move" for token in doc):
        new_folder_name = None
        parent_folder_name = None
        destination_folder_name = None
        for i, token in enumerate(doc):
            if token.text == "move":
                new_folder_name = doc[i + 1].text  # Assuming format: "move folder <foldername>"
            if token.text == "from":
                parent_folder_name = doc[i + 1].text  # Assuming format: "from <parentfoldername>"
            if token.text == "to":
                destination_folder_name = doc[i + 1].text  # Assuming format: "to <destinationfoldername>"
                break
        if new_folder_name and parent_folder_name and destination_folder_name:
            move_folder_from_input(new_folder_name, parent_folder_name, destination_folder_name)
            return "Moving folder..."
        else:
            print("Invalid input format.")
#run top command
    if any(token.text == "top" for token in doc):
        run_top_command(timeout=5)
    

    if any(token.text == "book" for token in doc) & any(token.text == "appointment" for token in doc):
        book()
        
        
    else:
        messages.append({"role": "user", "content": message})
        response = client.chat.completions.create(model="gpt-3.5-turbo", messages=messages)
        reply = response.choices[0].message.content
        messages.append({"role": "assistant", "content": reply})
        print("\n" + reply + "\n")
        return reply
    
    
messages = []
messages.append({"role": "system", "content": "You are an assistant that helps the user with his daily tasks and additionally you have the capability to navigate between apps. Communicate like talking to a friend. "})
greet()


while True:
    message = input()
    if message == "quit()":
        break
    response = process_message(message, messages)
    #print(messages)

