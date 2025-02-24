#include <iostream>  // For input-output operations
#include <vector>    // For using the vector container

using namespace std;

// Function to create the LPS (Longest Prefix Suffix) table
void computeLPSArray(string pattern, vector<int>& lps) {
    int length = 0; // Length of the previous longest prefix suffix
    lps[0] = 0; // The first value is always 0 since a single character has no proper prefix or suffix
    int i = 1; // Start checking from the second character

    while (i < pattern.size()) { // Loop through the pattern
        if (pattern[i] == pattern[length]) { // If characters match
            length++; // Increase the matched prefix length
            lps[i] = length; // Store the length in the LPS array
            i++; // Move to the next character
        } else { // If characters don't match
            if (length != 0) { // If we had a previous match
                length = lps[length - 1]; // Move back in the LPS array
            } else { // If no previous matches exist
                lps[i] = 0; // Set LPS value to 0
                i++; // Move to the next character
            }
        }
    }
}

// KMP Pattern Searching Algorithm
void KMPSearch(string text, string pattern) {
    int textLength = text.size(); // Get the length of the text
    int patternLength = pattern.size(); // Get the length of the pattern
    vector<int> lps(patternLength); // Create an LPS array for the pattern

    // Step 1: Compute the LPS array
    computeLPSArray(pattern, lps);

    int i = 0; // Index for iterating through the text
    int j = 0; // Index for iterating through the pattern

    while (i < textLength) { // Loop through the entire text
        if (pattern[j] == text[i]) { // If characters match
            i++; // Move to the next character in the text
            j++; // Move to the next character in the pattern
        }

        if (j == patternLength) { // If the full pattern is matched
            cout << "Pattern found at index " << i - j << endl; // Print the found index
            j = lps[j - 1]; // Use LPS to continue searching for other occurrences
        } else if (i < textLength && pattern[j] != text[i]) { // If a mismatch occurs
            if (j != 0) { // If we have a partial match
                j = lps[j - 1]; // Move back in the LPS array
            } else { // If no partial match exists
                i++; // Move to the next character in the text
            }
        }
    }
}

// Main Function
int main() {
    string text = "ABABDABACDABABCABAB"; // Define the text where we search
    string pattern = "ABABCABAB"; // Define the pattern to search for
    
    cout << "Searching for pattern '" << pattern << "' in text '" << text << "'...\n";
    
    KMPSearch(text, pattern); // Call KMP search function
    
    return 0; // Return 0 to indicate successful execution
}
