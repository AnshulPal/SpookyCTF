# Event Code Hunt (Forensics)

## Challenge Overview

I recently participated in a forensic challenge for the first time, and it was an incredible experience! The challenge involved analyzing event log files from a Windows system to investigate anomalies.

### Challenge Statement

Maya Elmer managed to seize one of The Consortium’s computers, but when she attempted to access a critical file, a sudden blue box flashed across her screen, and the file was instantly encrypted. Now, with the clock ticking, participants must decrypt the file and uncover the hidden contents. The Consortium's encryption is tough to crack, and only the most determined will succeed in revealing the secrets locked away within.

## Provided Files

![Event Log Files](Forensics/1.png)

## Investigation Process

### Step 1: Analyze PowerShell Files

I started by investigating the PowerShell files, as they can often contain useful information.

After reviewing the event logs, I found something particularly useful:

![PowerShell Command](Forensics/2.png)

This is a PowerShell command to run a script named `Chrome.py`, which takes the argument `Flag.txt` (potentially containing the flag) and uses the encryption key `I_Like_Big_Bytes_And_I_cannot_Lie!`.

### Step 2: Review Chrome.py Code

Next, I located the complete code for `Chrome.py`:

![Chrome.py Code](Forensics/3.png)

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
![](Forensics/4.png)


### Conclusion

This forensic challenge was a valuable experience, highlighting the importance of analyzing event logs and scripts in cybersecurity investigations. By understanding encryption, I was able to reverse the process and uncover the secrets locked away within the encrypted file.

![](Forensics/5.png)

