# The things should be imported
import pyttsx3
import speech_recognition as sr
import webbrowser
import os
import datetime
import pyjokes
import pywhatkit
# Initialize the speech engine
engine = pyttsx3.init()
voices =
engine.getProperty('voices')
engine.getProperty('voice',voices [1].id) # Used female voice
engine.setProperty("rate", 150)  # Adjust speaking speed
engine.setProperty("volume", 0.9)  # Adjust volume

# Function to make the AI speak
def speak(text):
    engine.say(text)
    engine.runAndWait()

# Function to take voice input from the user
def take_command():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    try:
        print("Recognizing...")
        command = recognizer.recognize_google(audio)
        print(f"You said: {command}")
        return command.lower()
    except sr.UnknownValueError:
        speak("Sorry, I did not understand that. Please say it again.")
        return None
    except sr.RequestError:
        speak("There seems to be an issue with the speech recognition service.")
        return None

# Function to handle user commands
def handle_command(command):
    if "time" in command:
        current_time = datetime.datetime.now().strftime("%I:%M %p")
        speak(f"The current time is {current_time}.")
    
    elif "open youtube" in command:
        speak("Opening YouTube.")
        webbrowser.open("https://www.youtube.com")
    
    elif "open google" in command:
        speak("Opening Google.")
        webbrowser.open("https://www.google.com")
    
    elif "search for" in command:
        search_query = command.replace("search for", "").strip()
        speak(f"Searching for {search_query}.")
        webbrowser.open(f"https://www.google.com/search?q={search_query}")
    
    elif "play music" in command:
        music_folder = "C:/Users/YourUsername/Music"  # Change this to your music folder path
        songs = os.listdir(music_folder)
        if songs:
            speak("Playing music.")
            os.startfile(os.path.join(music_folder, songs[0]))
        else:
            speak("Your music folder is empty.")
    
    elif "who are you" in command:
        speak("I am JARVIS, your personal assistant. How can I help you?")
    
    elif "exit" in command or "quit" in command:
        speak("Goodbye!")
        exit()
    
    else:
        speak("I'm sorry, I don't know how to do that yet.")

# Main loop
def main():
    speak("Hello, I am JARVIS. How can I assist you today?")
    while True:
        command = take_command()
        if command:
            handle_command(command)

if __name__ == "__main__":
    main()
