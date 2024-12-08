hama=input("daxel bka  :")

revers=hama[::-1]

print(revers)

#---------------------------- rail fence cipher


# Input plain text
plaintext=input("please enter word   :")
print(plaintext)

odd=""
even=""

for i in range (len(plaintext)):
    if i %2==0:
        even=even+plaintext[i]
    else:
        odd=odd+plaintext[i]
print(odd + "  "+even)


#----------------------------------- atbash cipher

# Define encoding and decoding arrays
enc = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
dec = ['z', 'y', 'x', 'w', 'v', 'u', 't', 's', 'r', 'q', 'p', 'o', 'n', 'm', 'l', 'k', 'j', 'i', 'h', 'g', 'f', 'e', 'd', 'c', 'b', 'a']
# Input plain text
text=input("please enter   :")
arr=list(text)

for i in range(len(arr)):
    for j in range(len(enc)):
        if arr[i]==enc[j]:
            print(dec[j] ,end=" ")

print()    

#--------------------------------------------
# Input plain text and set the key
key = 10
text = input("Enter Plain Text: ")
arr = list(text)  # Convert the input string to a list of characters

# Process each character
for i in range(len(arr)):
    # Get the position of the letter in the alphabet (1-based, a=1, b=2, ...)
    p = ord(arr[i]) - 96  # Convert character to its position
    p = (p + key) % 26 + 96  # Apply the shift and wrap around using modulo
    print(chr(p), end=" ")  # Convert back to character and print

print()  # Add a newline at the end
#-----------------------------------------------------------------------------------------------------------------

 #Text-Based CAPTCHA
import string
import random
def text_captcha():
    captcha=''.join(random.choices(string.ascii_letters + string.digits,k=5))
    print(f"The Captcha is: {captcha}")
    user_input=input("Please Enter the Text: ")
    if user_input==captcha:
        print("The captcha is passed.")
    else:
        print("CAPTCHA Failed. Try Again.")
text_captcha()
#-------------------------------------------------------------------------
#Math-Based CAPTCHA
def math_captcha():
    num1=random.randint(1,10)
    num2=random.randint(1,10)
    oparations=random.choice(['*','+','-'])
    captcha_text=f"{num1} {oparations} {num2}"
    answre=eval(captcha_text)
    print(f"Solve this equestion {captcha_text}")
    user_input=input("input the answer= ")
    if int(user_input)==answre:
        print("the captcha is passed")
    else:
        print("the captcha is failed. Try Again.")
math_captcha()
#--------------------------------------------------------------------------
#A Python Script That Deletes Files
import os

def delete_files_in_directory(directory):
    for filename in os.listdir(directory):
        file_path = os.path.join(directory, filename)
        # If it's a file, delete it
        if os.path.isfile(file_path):
            os.remove(file_path)
            print(f"Deleted file: {file_path}")
        # If it's a directory, go inside and delete files recursively
        elif os.path.isdir(file_path):
            delete_files_in_directory(file_path)

# Specify a directory to delete files in (CAUTION: Don't run this on a real directory)
directory_to_delete = "/path/to/some/directory"
delete_files_in_directory(directory_to_delete)
#--------------------------------------------------------------------------------
#Create a File and Write to It
import os

def create_and_write_file(directory, filename, content):
    # Create the full path to the file
    file_path = os.path.join(directory, filename)
    # Open the file in write mode (it will create the file if it doesn't exist)
    with open(file_path, "w") as file:
        file.write(content)
    print(f"Created and wrote to file: {file_path}")

# Specify the directory where you want to create the file
directory_to_create_file = "/path/to/some/directory"
# Create a file named 'example.txt' and write some content into it
create_and_write_file(directory_to_create_file, "example.txt", "This is some sample content.")

