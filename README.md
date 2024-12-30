# Subtitle-Translator-using-ZhipuAI
This Python script translates SRT (SubRip Subtitle) files from a source language to a target language using the ZhipuAI large language model (LLM). It reads an SRT file, extracts the text content, translates it using the glm-4-flash model from ZhipuAI, and then generates a new SRT file with the original and translated text.
Features:

SRT File Parsing: Accurately reads and parses SRT files, handling potential file not found and encoding errors.

Translation with ZhipuAI: Leverages the ZhipuAI API (specifically the glm-4-flash model) to perform high-quality text translation.

Customizable Language Support: Easily adapts to different source and target languages by modifying the source_language and target_language parameters (defaults to English-to-Chinese translation).

Error Handling: Includes robust error handling for file I/O operations, encoding issues, and potential API errors during translation.

Clear Output: Generates a new SRT file with a clear naming convention (<original_filename>翻译.srt) that includes both the original and the translated text for each subtitle entry.

Code Structure:

The script is organized into several functions, each responsible for a specific task:

read_srt(filepath):

Purpose: Reads an SRT file and extracts the subtitle entries (index, start time, end time, text).

Input: filepath (string): The path to the SRT file.

Output: A list of dictionaries, where each dictionary represents a subtitle entry. Returns None if the file is not found, has encoding problems, or is not a valid SRT file.

Error Handling: Catches FileNotFoundError and UnicodeDecodeError, printing informative error messages.

Regex: Uses a regular expression (r'(\d+)\n(\d{2}:\d{2}:\d{2},\d{3}) --> (\d{2}:\d{2}:\d{2},\d{3})\n(.*?)\n\n') to match and extract the components of each subtitle entry.

translate_text(text, source_language="en", target_language="zh"):

Purpose: Translates the given text using the ZhipuAI API.

Input:

text (string): The text to be translated.

source_language (string, optional): The source language code (default: "en").

target_language (string, optional): The target language code (default: "zh").

Output: The translated text (string) or None if an error occurs.

ZhipuAI API Interaction:

Constructs a prompt for the glm-4-flash model, defining the role of the model as a translation assistant.

Sends a request to the ZhipuAI API using client.chat.completions.create().

Sets top_p, temperature, and max_tokens to control the translation output.

Extracts the translated text from the API response.

Error Handling: Catches general exceptions during the API call and prints an error message.

generate_translated_srt(srt_entries, source_language="en", target_language="zh"):

Purpose: Iterates through the SRT entries, translates the text of each entry, and creates a new list of translated entries.

Input:

srt_entries (list): The list of original SRT entries.

source_language (string, optional): The source language code.

target_language (string, optional): The target language code.

Output: A list of dictionaries, similar to srt_entries, but with an added translated_text key containing the translated text.

Logic:

Calls translate_text() for each entry's text.

If the translation is successful, adds a new dictionary to the translated_entries list.

If a translation fails, prints an error message indicating the index of the failed entry.

write_srt(filepath, srt_entries):

Purpose: Writes the translated SRT entries to a new file.

Input:

filepath (string): The path to the output SRT file.

srt_entries (list): The list of translated SRT entries.

Output: Writes the SRT data to the specified file.

File Handling: Opens the output file in write mode ('w') with UTF-8 encoding.

Format: Writes the SRT entries in the correct format, including the index, start time, end time, original text, and translated text, separated by blank lines.

main():

Purpose: The main function that orchestrates the entire process.

Input: Prompts the user to enter the name of the SRT file.

Logic:

Gets the current working directory using os.getcwd().

Constructs the full file path.

Calls read_srt() to parse the SRT file.

Calls generate_translated_srt() to translate the entries.

Calls write_srt() to save the translated entries to a new file.

Prints a success message.

Dependencies:

re: Used for regular expression matching to parse the SRT file.

os: Used for file path manipulation (getting the current working directory, splitting filenames).

zhipuai: The official Python client library for interacting with the ZhipuAI API. You'll need to install it using: pip install zhipuai

Setup and Usage:

Install the ZhipuAI library:

pip install zhipuai
Use code with caution.
Bash
Obtain a ZhipuAI API Key:

Sign up or log in to your ZhipuAI account.

Go to the API key management section.

Generate a new API key.

Replace the Placeholder API Key:

In the script, replace "****" in the line client = ZhipuAI(api_key="****") with your actual API key.

Place the SRT File:

Put the SRT file you want to translate in the same directory as the Python script.

Run the Script:

Execute the script from your terminal: python your_script_name.py

The script will prompt you to enter the name of the SRT file.

Output:

A new SRT file will be created in the same directory, named <original_filename>翻译.srt, containing the original and translated subtitles.

Further Improvements:

Batch Processing: Modify the script to handle multiple SRT files at once.

GUI: Create a graphical user interface for a more user-friendly experience.

Advanced Translation Options: Expose more ZhipuAI API parameters (e.g., top_k, different models) to allow users to fine-tune the translation process.

More Comprehensive Error Handling: Implement more specific error handling for different types of API errors.

Logging: Add logging to track the translation process and any errors that occur.
