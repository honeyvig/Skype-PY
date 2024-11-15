# Skype-PY
Accessing Skype programmatically, especially its chats, contacts, and profile information, is a bit more complex than other messaging platforms because Skype does not provide an official API for direct access to user data like messages and contacts. However, there are a few workarounds that can be used, such as:

    Skype Web API (Unofficial): There are unofficial APIs that can interact with Skype, but they are often deprecated or unreliable.

    Microsoft Graph API: This API gives access to Microsoft services, including Skype for Business data. However, this only applies to Skype for Business (formerly Lync) users, not regular Skype users.

    Skype Desktop Client Automation: You can use tools like Sikuli or PyAutoGUI to automate the Skype desktop client. This can simulate user actions like reading messages, extracting contact information, etc.

    Skype's skype4py Library: This is an unofficial library that allows some interaction with Skype, such as sending messages, retrieving contacts, etc.

Let’s go over two main approaches here:
1. Using Skype4Py (Unofficial API)

Skype4Py is an old, unofficial Python library that can be used to interact with the Skype Desktop client. The library allows you to access Skype contacts, send messages, and read chat history. However, Skype4Py is no longer actively maintained, and may not work with modern versions of Skype.

Steps to Install Skype4Py:

    Install Skype4Py (if available):

    pip install Skype4Py

    Ensure that you have Skype Desktop Client installed and running.

    You can interact with Skype by sending commands and retrieving contact and chat information.

Here is an example using Skype4Py:

import Skype4Py

# Initialize Skype instance
skype = Skype4Py.Skype()

# Check if Skype is running
if skype.Client.IsRunning:
    skype.Attach()

# Get all contacts
contacts = skype.GetContacts()

# Display contact names and their online status
for contact in contacts:
    print(f"Contact: {contact.DisplayName}, Online: {contact.OnlineStatus}")

# Get recent chats
chats = skype.GetChats()
for chat in chats:
    print(f"Chat with: {chat.Name}")
    for message in chat.Messages:
        print(f"Message: {message.Body}")

# Get your own profile information
profile = skype.Profile
print(f"Your Skype name: {profile.SkypeName}")
print(f"Your status message: {profile.StatusMessage}")

Limitations:

    It only works with the desktop version of Skype.
    Not all features are available, and the library is outdated.
    Skype's desktop client might require you to log in manually for the script to interact with it.

2. Using Microsoft Graph API (For Skype for Business)

If you're working with Skype for Business, you can use the Microsoft Graph API to access Skype-related data (e.g., contacts, messages, meetings).
Steps to Use Microsoft Graph API:

    Register an Application in the Azure portal to get API credentials (Client ID, Secret, Tenant ID).
    Authenticate using OAuth2.
    Use Microsoft Graph API to retrieve Skype data.

Install required libraries:

pip install requests
pip install msal  # For authentication

Example to Fetch Profile Information and Contacts:

import requests
import msal

# Constants
TENANT_ID = 'your-tenant-id'
CLIENT_ID = 'your-client-id'
CLIENT_SECRET = 'your-client-secret'
SCOPES = ['https://graph.microsoft.com/.default']

# Create a confidential client application
app = msal.ConfidentialClientApplication(
    CLIENT_ID,
    authority=f'https://login.microsoftonline.com/{TENANT_ID}',
    client_credential=CLIENT_SECRET,
)

# Get an access token
token_response = app.acquire_token_for_client(scopes=SCOPES)

# Get the access token from the response
access_token = token_response.get('access_token')

# Fetch user profile info (Example: Get the signed-in user's profile)
profile_url = 'https://graph.microsoft.com/v1.0/me'
response = requests.get(
    profile_url,
    headers={'Authorization': f'Bearer {access_token}'}
)

profile_data = response.json()
print("Profile Info:", profile_data)

# Fetch the user's contacts (Example: Get user contacts)
contacts_url = 'https://graph.microsoft.com/v1.0/me/contacts'
response = requests.get(
    contacts_url,
    headers={'Authorization': f'Bearer {access_token}'}
)

contacts_data = response.json()
print("Contacts:", contacts_data)

Important Notes:

    The above code assumes you have registered your app in Azure and granted it the necessary API permissions.
    This works only for Skype for Business or Teams (part of Microsoft 365) accounts, not regular Skype accounts.
    To access Skype data, your account must have the necessary permissions (such as user.read, user.read.all, contact.read).

3. Automating Skype Desktop with PyAutoGUI or Sikuli

If you're trying to interact with the Skype Desktop client to access chats, contacts, etc., without using an official API, you can automate GUI interactions using PyAutoGUI or Sikuli.

Here’s a basic example using PyAutoGUI to click and type in the Skype desktop client:

Install PyAutoGUI:

pip install pyautogui

Basic Automation Script:

import pyautogui
import time

# Open Skype if it's not already open
# pyautogui.hotkey('win', 's')  # Windows + S to open search
# pyautogui.typewrite('Skype')  # Type 'Skype' in the search box
# pyautogui.press('enter')  # Press Enter to open Skype

# Give it a few seconds to open Skype
time.sleep(5)

# Example: Click on a chat (coordinates need to be adjusted)
pyautogui.click(x=500, y=300)  # Adjust the coordinates based on the screen resolution

# Type a message in the chat
pyautogui.typewrite("Hello, this is an automated message!")
pyautogui.press('enter')

# Screenshot the current window
pyautogui.screenshot('skype_chat_screenshot.png')

Limitations:

    This is a very basic form of automation.
    Requires manual setup and testing.
    Limited by the UI elements on the screen (i.e., if Skype UI changes, the coordinates need to be updated).

Conclusion

    Skype4Py can be used if you are still working with older Skype versions and are willing to handle its limitations.
    Microsoft Graph API is the official way to access Skype for Business or Microsoft Teams data, but it does not apply to regular Skype users.
    Automation tools like PyAutoGUI or Sikuli can be used if you're willing to automate interactions with the Skype desktop client, but it's less reliable and can break with updates to Skype.
