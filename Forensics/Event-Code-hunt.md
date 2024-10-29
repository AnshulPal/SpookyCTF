# Event Code Hunt (Forensics)

## Challenge Overview

I recently participated in a forensic challenge for the first time, and it was an incredible experience! The challenge involved analyzing event log files from a Windows system to investigate anomalies.

### Challenge Statement

Maya Elmer managed to seize one of The Consortium’s computers, but when she attempted to access a critical file, a sudden blue box flashed across her screen, and the file was instantly encrypted. Now, with the clock ticking, participants must decrypt the file and uncover the hidden contents. The Consortium's encryption is tough to crack, and only the most determined will succeed in revealing the secrets locked away within.

## Provided Files

![1](https://github.com/user-attachments/assets/78c26fc2-acc5-4bc6-ae44-b3d4e2b1c210)

## Investigation Process

### Step 1: Analyze PowerShell Files

I started by investigating the PowerShell files, as they can often contain useful information.

After reviewing the event logs, I found something particularly useful:

![2](https://github.com/user-attachments/assets/ee26ef1c-cf45-40d9-b644-333762e553ea)

This is a PowerShell command to run a script named `Chrome.py`, which takes the argument `Flag.txt` (potentially containing the flag) and uses the encryption key `I_Like_Big_Bytes_And_I_cannot_Lie!`.

### Step 2: Review Chrome.py Code

Next, I located the complete code for `Chrome.py`:

![3](https://github.com/user-attachments/assets/6de027d8-a8aa-462e-b60c-a0f510a48c12)

Here’s the relevant code snippet:

```python
import sys

def process_data(input_bytes, key):
    key_bytes = key.encode('utf-8')
    return bytearray([b ^ key_bytes[i % len(key_bytes)] for i, b in enumerate(input_bytes)])

def main():
    if len(sys.argv) != 4:
        print('Usage: python script.py <input_file> <output_file> <key>')
        return
    
    input_file = sys.argv[1]
    output_file = sys.argv[2]
    key = sys.argv[3]

    with open(input_file, 'rb') as f:
        input_data = f.read()

    # Call the same function to decode the data
    result_data = process_data(input_data, key)

    with open(output_file, 'wb') as f:
        f.write(result_data)

if __name__ == '__main__':
    main()
```
### Step 3: Reverse the Code for Decryption

To retrieve the flag, I modified the code to decrypt the file instead of encrypting it.

### Step 4: Run the Decryption Script

After making the necessary adjustments, I executed the script with the encrypted file and the key:

```bash
python Chrome.py encrypted_file output_file I_Like_Big_Bytes_And_I_cannot_Lie!
```
### Result: The Flag

Upon running the script, I successfully decrypted the file and revealed the hidden flag.
![4](https://github.com/user-attachments/assets/ac9b899a-b721-4b53-8f89-e2b68c0d617a)


### Conclusion

This forensic challenge was a valuable experience, highlighting the importance of analyzing event logs and scripts in cybersecurity investigations. By understanding encryption, I was able to reverse the process and uncover the secrets locked away within the encrypted file.

![5](https://github.com/user-attachments/assets/3901e9a5-8b3a-4bc7-b3ed-7ebe36087bbc)

