# Geometry Dash Custom Song Adder

This repository contains a tool that automates the process of adding custom songs to **Geometry Dash**. The program allows you to add custom MP3 songs by generating unique song IDs automatically.

## Features
- Automatically generates unique song IDs.
- Adds custom MP3 songs to the Geometry Dash directory.
- Easy-to-use interface with a file selection dialog.

## How to Use
1. Download the `.exe` file from the repository.
2. Run the program and select an MP3 file.
3. The program will automatically generate a unique song ID and add the song to your Geometry Dash directory.

## License
This project is **not licensed**. You are free to view the code, but **modification and redistribution are not permitted**.

## Disclaimer
This tool is for personal use only and is not officially supported by the developers of Geometry Dash. Use it at your own risk.

Q/A:

Q: "Why is this being flagged as a virus in virus total?"
A: Look below, also the code is available to look into if your worried

Why is My File Flagged as Malicious?
SecureAge - Malicious

Why flagged: It modifies system files, often flagged by antivirus.
Why it’s safe: It only adds MP3 files to Geometry Dash, with no harm to the system.
SentinelOne (Static ML) - Suspicious PE

Why flagged: Custom software patterns can seem suspicious.
Why it’s safe: It doesn’t perform any harmful actions.
Skyhigh (SWG) - BehavesLike.Win64.Dropper.tc

Why flagged: Modifies system files, similar to malware droppers.
Why it’s safe: It only adds songs to Geometry Dash, no harmful files.
Zillya - Trojan.Agent.Win32.4064176

Why flagged: It modifies system files like a Trojan.
Why it’s safe: There’s no data-stealing or harmful payload, just song addition.
________________________________________________________________________________________
Code:

import os
import random
import shutil
import tkinter as tk
from tkinter import filedialog, messagebox

def generate_random_id(song_directory):
    # Extract existing IDs from valid file names
    existing_ids = []
    for f in os.listdir(song_directory):
        if f.endswith('.mp3'):
            try:
                existing_ids.append(int(f.split('.')[0]))
            except ValueError:
                # Skip files that do not match the expected format
                continue

    # Generate a random unused ID
    while True:
        new_id = random.randint(100000, 999999)
        if new_id not in existing_ids:
            return new_id

def add_song():
    # Open file dialog to select MP3 file
    file_path = filedialog.askopenfilename(filetypes=[("MP3 Files", "*.mp3")])
    if not file_path:
        messagebox.showwarning("Warning", "No file selected. Please select an MP3 file.")
        return

    # Automatically detect the Geometry Dash directory under the AppData
    song_directory = os.path.join(os.getenv('APPDATA'), 'GeometryDash')
    if not os.path.exists(song_directory):
        # If GeometryDash is not found in APPDATA, try LOCALAPPDATA
        song_directory = os.path.join(os.getenv('LOCALAPPDATA'), 'GeometryDash')

    print(f"Song directory: {song_directory}")

    # Check if the directory exists and is writable
    if not os.path.exists(song_directory):
        messagebox.showerror("Error", f"{song_directory} directory not found.")
        return
    if not os.access(song_directory, os.W_OK):
        messagebox.showerror("Error", f"Cannot write to {song_directory}.")
        return

    try:
        print(f"Selected file: {file_path}")
        
        # Extract existing IDs from valid file names
        existing_ids = []
        for f in os.listdir(song_directory):
            if f.endswith('.mp3'):
                try:
                    existing_ids.append(int(f.split('.')[0]))
                except ValueError:
                    continue

        # Generate a random unused song ID
        new_id = generate_random_id(song_directory)
        print(f"Generated ID: {new_id}")

        # Construct the new file name and path
        new_file_name = f"{new_id}.mp3"
        new_file_path = os.path.join(song_directory, new_file_name)
        print(f"New file path: {new_file_path}")

        # Ensure the file is being copied
        shutil.copy(file_path, new_file_path)
        print(f"File copied successfully to: {new_file_path}")

        messagebox.showinfo("Success", f"Song added successfully!\nSong ID: {new_id}\nFile Name: {new_file_name}")
    except Exception as e:
        messagebox.showerror("Error", f"An error occurred: {e}")
        print(f"Error: {e}")

# Create the main window
root = tk.Tk()
root.title("Geometry Dash Custom Song Adder")

# Create a button to add songs
add_song_button = tk.Button(root, text="Add Song", command=add_song, font=("Arial", 14), padx=20, pady=10)
add_song_button.pack(pady=20)

# Run the application
root.mainloop()
