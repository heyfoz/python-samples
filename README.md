# python_samples
Samples of Python 3 applications on Mac, utilizing object-oriented development principles and line commenting. A text file is provided in the repository, but the contents are also displayed below:

#!/usr/bin/env python3
# Forrest Moulin
# Resume Helper is a python GUI application developed to facilitate the retrieval and copying of local resume files (txt, json) on Mac

from tkinter import *         # Import * from tkinter module
from tkinter import ttk       # Import ttk from tkinter module
import json                   # Import JSON module
from datetime import datetime # datetime is used to print the time that that the button was selected and the timestamp for file naming
import os                     # Import os to use terminal commands to move files to different directories
import getpass                # getpass is used to get username to facilitate creating and reading file names
import shutil                 # shutil module offers high-level file operations used to copy txt file contents

class ResumeHelper:           # Class definition
    
    def __init__(self, master): # Define __init__ method with self/master parameters           

        self.master = master # Used to manage tk window with other functions

        # File location object initialization
        self.user_name = getpass.getuser() # Get user name using getuser function from getpass module
        # Make mac user directory string that can be read using with open 
        self.mac_user_dir = '/Users/' + self.user_name 
        
        # Flat file name with .txt extension of read/copied backup file
        self.backup_text_file_name = 'Forrest_Moulin_Flat_Resume.txt' 
        # File directory used to locate backup text file
        self.backup_text_file_directory = self.mac_user_dir + '/BackupText/Directory/Path/' 
        # Designate backup text file location
        self.backup_text_file_pathname = self.backup_text_file_directory + self.backup_text_file_name 
        
        # Text file copy directory location
        self.text_file_copy_directory = self.mac_user_dir + '/TextCopy/Directory/Path/' 
            
        self.backup_json_file_name = 'Forrest_Moulin_Resume.json' # JSON file name of read/copied file
        # File directory used to locate backup json file
        self.backup_json_file_directory = self.mac_user_dir + '/BackupText/Directory/Path/' 
        # Designate json file location
        self.backup_json_file_pathname = self.backup_json_file_directory + self.backup_json_file_name 
        
        # JSON file copy directory location
        self.json_file_copy_directory = self.mac_user_dir + '/BackupJSON/Directory/Path/' 
        
        self.python_home_directory = self.mac_user_dir + '/Path/To/PythonHome/' # Python home directory location
        # Designate location of profile photo image
        self.profile_image_file_pathname = self.mac_user_dir + '/Path/To/Profile_Photo.png' 
 
        self.copy_text_count = int() # count variable implemented to help button listening without creating additional confirm button/logic
        self.copy_json_count = int() # count variable implemented to help button listening without creating additional confirm button/logic
                
        # First frame named header_frame_0
        self.header_frame_0 = ttk.Frame(master) # Create Frame using master as parent
        self.header_frame_0.pack(pady = 3) # Pack header_frame_0 into the top level window
        self.header_text_0 = ' Forrest Moulin '  # Used to assign text value to ttk Label 0 below
        
        # Overview Label (0)
        self.header_label_0 = ttk.Label(master, text = self.header_text_0, borderwidth = 1, relief = 'solid')
        self.header_label_0.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Profile label (1)
        self.header_label_1 = ttk.Label(master, text = ' 1. Profile ') # Create ttk label using master as parent
        self.header_label_1.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Academics label (2)
        self.header_label_2 = ttk.Label(master, text = ' 2. Academics ') # Create ttk label using master as parent
        self.header_label_2.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Awards label (3)
        self.header_label_3 = ttk.Label(master, text = ' 3. Awards ') # Create ttk label using master as parent
        self.header_label_3.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Professional Skills label (4)
        self.header_label_4 = ttk.Label(master, text = ' 4. Professional Skills ') # Create ttk label using master as parent
        self.header_label_4.pack(pady = 10) # Use geometry manager to pack the label into the top level window
        
        # Professional Experience label (5)
        self.header_label_5 = ttk.Label(master, text = ' 5. Professional Experience ') # Create ttk label using master as parent
        self.header_label_5.pack(pady = 10) # Use geometry manager to pack the label into the top level window
        
        # Create open file frame/buttons
        self.button_frame = ttk.Frame(master) # Frame that contains buttons
        self.button_frame.pack(pady = 10) # Use geometry manager to pack the button_frame into the top level window
        
        # Open Text File Button
        self.open_text_file_button = (
            ttk.Button(self.button_frame, text = 'Open Resume Text File', command = self.open_text_file_resume, width = 20))
        self.open_text_file_button.grid(row = 0, column = 0, padx = 5, pady = 5)
        
        # Open JSON File Buton
        self.open_json_file_button = (
            ttk.Button(self.button_frame, text = 'Open Resume JSON File', command = self.open_json_file_resume, width = 20))
        self.open_json_file_button.grid(row = 0, column = 1, padx = 5, pady = 5)
        
        # Copy Text File Button
        self.copy_text_file_resume_button = (
            ttk.Button(self.button_frame, text = 'Copy Text File Resume', command = self.copy_text_file_resume, width = 20))
        # grid function to place button is specific row-column pair
        self.copy_text_file_resume_button.grid(row = 1, column = 0, padx = 5, pady = 7) 
        
        # Copy JSON File Buton
        self.copy_json_file_resume_button = (
            ttk.Button(self.button_frame, text = 'Copy JSON File Resume', command = self.copy_json_file_resume, width = 20))
        # grid function to place button is specific row-column pair
        self.copy_json_file_resume_button.grid(row = 1, column = 1, padx = 5, pady = 7) 
        
        # Tk window profile image
        self.img = PhotoImage(file = self.profile_image_file_pathname) # Use PhotoIMage to load image onto Tk window
        Label(master,image = self.img).pack(pady = 10) # Pack the image into the Tk window
        
        # Message label/greeting
        self.message_greeting = StringVar() # String variable for message greeting text
        self.message_greeting = ' Welcome to Resume Helper! Please select a button to get started. '
        # Create ttk Label using master as parent
        self.message_label = ttk.Label(master, text = self.message_greeting, wraplength=700, justify=CENTER) 
        self.message_label.pack(pady = 10) # Pack message label into top level window
        
    def open_text_file_resume(self): # Define open text file function
        print('Open Resume text File button selected:') # Visibility of system status
        self.create_timestamp() # Timestamp metadata collection
        os.system('open -a TextEdit ' + self.backup_text_file_pathname) # Run terminal command to open the text file with Text Edit 
        
    def copy_text_file_resume(self): # Define copy text file function
        print('Copy Text File Resume button selected:') # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata creation and collection   
        if self.copy_text_count != 1: # If the copy text count variable does not equal 1 (ready for copying)
            self.reset_listeners() # Clear all button listener variables (set both to 0)
            self.copy_text_count = 1 # Set copy text count variable to 1, allowing for file to be copied with same button on next click
            self.read_file(self.backup_text_file_pathname) # Read the backup text file
            print(self.data) # Print data read from the text file for debugging
            self.reduce_window_size(800, 800) # Reduce window size so console details are visibile on button click
            # Are you sure function to confirm that user would like to create a file copy 
            self.are_you_sure('text')
        else:
            text_file_copy_name = self.date_time_string + 'Forrest_Moulin_Resume.txt' # Assign a new text file name
            print('Creating ' + text_file_copy_name + '...') # Confirmation of new file name
            # Use shutil module to copy contents of file to a new file
            shutil.copyfile(self.backup_text_file_pathname, self.text_file_copy_directory + text_file_copy_name) 
            file_size = os.path.getsize(self.text_file_copy_directory + text_file_copy_name) # Document file size in bytes
            print('File size: '+str(file_size)+' bytes') # Print file size metadata to console
            label_text = 'Text file created:\n\'' + self.text_file_copy_directory + text_file_copy_name + '\'\n'
            print(label_text) # Visibility of system status in console
            self.message_label.config(text = label_text) # Update message label text
            self.reset_listeners() # Clear all button listener variables (set both to 0)
        
    def open_json_file_resume(self): # Define open json file function
        print('Open JSON File Resume button selected:') # Visibility of system status
        self.create_timestamp() # Debugging/visibility of system status
        # Run terminal command to open the JSON file with TextEdit on Mac
        os.system('open -a TextEdit ' + self.backup_json_file_pathname) 
 
    def copy_json_file_resume(self): # Define copy json file function
        print('Copy JSON File Resume button selected:') # Visilbity of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        if self.copy_json_count != 1: # If the copy json count variable does not equal 1 (ready for copying)
#             self.reset_listeners() # Clear all button listener variables (set both to 0)
            self.copy_json_count = 1 # Set copy json count variable to 1, allowing for file to be copied with same button on next click
            self.read_file(self.backup_json_file_pathname) # Call read_file function using backup json file pathname as parameter
            print(json.dumps(self.data) + '\n') # Print data dictionary for debugging
            self.reduce_window_size(800, 800) # Reduce window size so console details are visibile on button click
            # Are you sure function to confirm that user would like to create a file copy 
            self.are_you_sure('json')
        else:
            json_file_copy_name = self.date_time_string + 'Forrest_Moulin_Resume.json' # Assign new JSON file name        
            print('Creating ' + json_file_copy_name + '...')  # Confirmation of new file name
            # Use shutil module to copy contents to a new file
            shutil.copyfile(self.backup_json_file_pathname, self.json_file_copy_directory + json_file_copy_name) 
            file_size = os.path.getsize(self.text_file_copy_directory + json_file_copy_name) # Document file size in bytes
            print('File size: '+str(file_size)+' bytes') # Print file size metadata to console        
            label_text = 'JSON file created\n' + self.json_file_copy_directory + json_file_copy_name + '\n'
            print(label_text) # Visibility of system status in console
            self.message_label.config(text = label_text) # Update message label text
#             self.reset_listeners() # Clear all button listener variables (set both to 0)
        self.reset_listeners() # Clear all button listeners (set both to 0)

    def create_timestamp (self):        # Define function to capture time and date (used in file naming)
        self.timestamp = datetime.now() # Capture timestamp using now function from datetime module
        # Create str that displays date using . separators in YYYY.MM.DD format
        self.date = str(self.timestamp.strftime('%Y.%m.%d')) 
        self.time = str(datetime.now().time())[:-7]     # time str used for timestamp on file name
        self.time = self.time.replace(':', '.')         # Replace timestamp colons with . for file name formatting
        self.date_time_string = self.date + '_' + self.time + '_' # Facilitate file naming
        print('date_time_string: ' + self.date_time_string)  # Print results for debugging
        print('Date: '+self.date+', Time: '+self.time+'\n')  
        
    def read_file(self, filepath): # Define function to read file based on pathname parameter
        self.filepath = filepath   # Define filepath variable using self
        with open(self.filepath, 'r') as file: # Read the text/json file according to the file path
            data = file.read() # Assign value of file contents to data dictionary variable
            self.data = data   # Declare self.data variable equal to data variable for use outside with open
        
    def set_geometry_string(self, width, height, left, top): # Define mutator function
        self.geometry_string = str(width) + 'x' + str(height) + '+' + str(left) + '+' + str(top)
        self.tk_window_width = width
        self.tk_window_height = height
        self.tk_window_left_position = left
        self.tk_window_top_position = top
        
    def get_geometry_string(self): # Define accessor function
        return self.geometry_string
        
    def set_max_screen_dimensions(self, width, height): # Define max screen dimensions mutator function
        self.max_screen_width = width
        self.max_screen_heigth = height
        
    def reduce_window_size(self, width, height): # Define reduce window size function used to manage tk window size
        self.tk_window_width = width # Reduce window width
        self.tk_window_height = height # Reduce window height
        # When file is copied, reduce window size so that details printed to the console are visible. Position in center of display
        self.set_geometry_string(self.tk_window_width, self.tk_window_height,
                                 int((self.max_screen_width-self.tk_window_width)/2), 0)
        self.master.geometry(self.geometry_string) # Update window geometry
    
    def set_tk_window_left_position(self, left): # Define tk window left position mutator function
        self.tk_window_left_position = int(left)
        
    def get_tk_window_left_position(self): # Define tk window left position accessor function
        return self.tk_window_left_position
        
    def set_tk_window_top_position(self, top): # Define tk window top position mutator function
        self.tk_window_top_position = int(top)
        
    def get_tk_window_top_position(self): # Define tk window top position accessor function
        return self.tk_window_top_position
    
    def set_tk_window_positions(self, left, top): # Define tk window positions mutator function
        self.tk_window_left_position = int(left)
        self.tk_window_top_position = int(top)
    
    def get_tk_window_width(self): # Define tk window width acessor function
        return self.tk_window_width
    
    def set_tk_window_width(self, width): # Define tk window width mutator function
        self.tk_window_width = int(width) # Return window width as integer
        
    def get_tk_window_height(self): # Define tk window width acessor function
        return self.tk_window_height
    
    def set_tk_window_height(self, height): # Define tk window width mutator function
        self.tk_window_height = int(height) # Return window height as integer
    
    def are_you_sure(self, filetype):
        pathname_var = str() # String object for pathname
        if filetype == 'json': # If file type is json, use json file path
            pathname_var = self.backup_json_file_pathname
            self.copy_json_count = 1 # Update count variable to allow file to be copied on next button click
        else: # Else if file type is text, use text file path
            pathname_var = self.backup_text_file_pathname
            self.copy_text_count = 1 # Update count variable to allow file to be copied on next button click
        label_text = ('Are you sure you would like to copy\n\'' + pathname_var +
            '\'?\nSelect the highlighted button a second time to create a copy of the ' + filetype + ' file.')
        self.message_label.config(text = label_text) # Update message label text
        
    def reset_listeners(self): # Define function to reset button listener variables to 0
        self.copy_json_count = 0
        self.copy_text_count = 0
    
def main():     # Define main method
    root = Tk() # Top level root window created with Tk constructor
    # Declare object resume_helper of class ResumeHelper and use root parameter to initialize the object    
    resume_helper = ResumeHelper(root) 
    print('Resume Helper initiated! :)') # Visibility of application startup
    resume_helper.create_timestamp() # Test create_timestamp function when main method is run and create date_time_string object
    root.title('Resume Helper!')  # Set root/tk window title
    
    # Format GUI window size to full screen width/height   
    resume_helper.set_tk_window_width(root.winfo_screenwidth()) # Define tk window width using the winfo display dimensions
    resume_helper.set_tk_window_height(root.winfo_screenheight()) # Define tk window height using the winfo display dimensions
    # Set screen width/height as max display size
    resume_helper.set_max_screen_dimensions(root.winfo_screenwidth(), root.winfo_screenheight()) 
    resume_helper.set_tk_window_positions(0, 0)
    # Concatenate geometry string in 'widthxheight+left_position+top_position' format
    resume_helper.set_geometry_string(resume_helper.get_tk_window_width(), resume_helper.get_tk_window_height(),
                                      resume_helper.get_tk_window_left_position(), resume_helper.get_tk_window_top_position())
    root.geometry(resume_helper.get_geometry_string()) # Use geometry string value as argument to format tk window size/position
    # Debugging and visibility of GUI metadata
    print('Window Left Position: '+str(resume_helper.get_tk_window_left_position()))
    print('Window Top Position: '+str(resume_helper.get_tk_window_top_position()))
    print('Display winfo width: '+str(resume_helper.get_tk_window_width()))
    print('Display winfo height: '+str(resume_helper.get_tk_window_height()))    
    root.mainloop()                # Used to loop the application in order to monitor for command events
    
if __name__ == '__main__': main()  # If __name__ variable equals '__main__', call the main method

# End of resume_helper.py
