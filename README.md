# File-organizer
Script to automatically sort files into folders based on their extensions, names, or creation dates.
import os
import shutil
import logging
from datetime import datetime

def setup_logging():
    logging.basicConfig(filename="file_organizer.log", level=logging.INFO,
                        format="%(asctime)s - %(levelname)s - %(message)s")

def create_folder(directory):
    if not os.path.exists(directory):
        os.makedirs(directory)

def organize_by_extension(source_folder):
    for filename in os.listdir(source_folder):
        file_path = os.path.join(source_folder, filename)
        if os.path.isfile(file_path):
            ext = os.path.splitext(filename)[1][1:].lower()
            if ext:
                folder_path = os.path.join(source_folder, ext.upper())
                create_folder(folder_path)
                shutil.move(file_path, os.path.join(folder_path, filename))
                logging.info(f"Moved {filename} to {folder_path}")

def organize_by_name(source_folder):
    for filename in os.listdir(source_folder):
        file_path = os.path.join(source_folder, filename)
        if os.path.isfile(file_path):
            first_letter = filename[0].upper()
            folder_path = os.path.join(source_folder, first_letter)
            create_folder(folder_path)
            shutil.move(file_path, os.path.join(folder_path, filename))
            logging.info(f"Moved {filename} to {folder_path}")

def organize_by_date(source_folder):
    for filename in os.listdir(source_folder):
        file_path = os.path.join(source_folder, filename)
        if os.path.isfile(file_path):
            creation_time = os.path.getctime(file_path)
            date_folder = datetime.fromtimestamp(creation_time).strftime('%Y-%m-%d')
            folder_path = os.path.join(source_folder, date_folder)
            create_folder(folder_path)
            shutil.move(file_path, os.path.join(folder_path, filename))
            logging.info(f"Moved {filename} to {folder_path}")

def main():
    setup_logging()
    source_folder = input("Enter the folder path to organize: ")
    print("Choose sorting method:")
    print("1. By Extension")
    print("2. By Name")
    print("3. By Creation Date")
    choice = input("Enter choice (1/2/3): ")
    
    if choice == "1":
        organize_by_extension(source_folder)
    elif choice == "2":
        organize_by_name(source_folder)
    elif choice == "3":
        organize_by_date(source_folder)
    else:
        print("Invalid choice.")
        logging.warning("Invalid sorting method chosen.")

if __name__ == "__main__":
    main()

