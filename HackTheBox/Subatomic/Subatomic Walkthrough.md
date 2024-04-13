# Subatomic Walkthrough

> **💡 Forela is in need of your assistance. They were informed by an employee that their Discord account had been used to send a message with a link to a file they suspect is malware. The message read: "Hi! I've been working on a new game I think you may be interested in it. It combines a number of games we like to play together, check it out!". The Forela user has tried to secure their Discord account, but somehow the messages keep being sent and they need your help to understand this malware and regain control of their account! Warning This is a warning that this Sherlock includes software that is going to interact with your computer and files. This software has been intentionally included for educational purposes and is NOT intended to be executed or used otherwise. Always handle such files in isolated, controlled, and secure environments. One the Sherlock zip has been unzipped, you will find a DANGER.txt file. Please read this to proceed.**
> 

**Task 1**

**What is the Imphash of this malware installer?**

(Can be found using Virustotal Details)

![Untitled](Subatomic%20Walkthrough/Untitled.png)

**Task 2**

**The malware contains a digital signature. What is the program name specified in the `SpcSpOpusInfo` Data Structure?**

> 💡 `SpcSpOpusInfo` is a data structure in the Authenticode signature format that can contain information about the software publisher and the software's program name. It is used for signing and verifying software in Windows.
> 
- Can be found in Virustotal Details

![Untitled](Subatomic%20Walkthrough/Untitled%201.png)

**Task 3**

**The malware uses a unique GUID during installation, what is this GUID?**

(Can be found using procdot)

![Untitled](Subatomic%20Walkthrough/Untitled%202.png)

**Task 4**

**The malware contains a package.json file with metadata associated with it. What is the 'License' tied to this malware?**

- Can be found by filtering for “License” in procdot, after doing so, I found the files below, I didn’t find the answer I was looking for in “***LICENSE.electron.txt”*** although in “***LICENSES.chromium.html”*** there are over 25K references to the word “license”. I am most certainly not searching through 25K references to the word “license”, instead I asked ChatGPT *“What are common software licenses?”* and I searched for each result that it returned and eventually found the correct answer.

Here's an overview of some common software licenses, each catering to different types of projects and legal requirements:

1. **MIT License (MIT):**
    - **Type:** Permissive
    - **Key Characteristics:** Allows reuse within proprietary software as long as the license is included. Very minimal restrictions on how the software can be used, modified, and distributed.
2. **GNU General Public License (GPL):**
    - **Type:** Copyleft
    - **Key Characteristics:** Requires that modified versions of licensed software must also be distributed under the same GPL license. Ensures that the freedoms to use, modify, and redistribute are preserved in all versions of the software.
3. **Apache License 2.0:**
    - **Type:** Permissive
    - **Key Characteristics:** Similar to the MIT License but also provides an express grant of patent rights from contributors to users.
4. **BSD Licenses:**
    - **Type:** Permissive
    - **Versions:** 2-clause (Simplified BSD License), 3-clause (New BSD License), 4-clause (Original BSD License)
    - **Key Characteristics:** Allows for redistribution and use in source and binary forms, with or without modification. The 3-clause license includes a non-endorsement clause.
5. **Creative Commons Licenses:**
    - **Type:** Varied (from permissive to copyleft)
    - **Key Characteristics:** A set of licenses that enable creators to communicate which rights they reserve and which rights they waive for the benefit of recipients or other creators. Not typically used for software.
6. **GNU Lesser General Public License (LGPL):**
    - **Type:** Copyleft (weaker than GPL)
    - **Key Characteristics:** Allows linking of licensed libraries with proprietary software, provided that modifications to the LGPL-covered components are released under the LGPL.
7. **Mozilla Public License 2.0 (MPL):**
    - **Type:** Weak copyleft
    - **Key Characteristics:** Requires that modifications to licensed code be made available under the MPL, but allows the inclusion of MPL-licensed code in proprietary projects under certain conditions.
8. **Eclipse Public License 1.0 (EPL):**
    - **Type:** Weak copyleft
    - **Key Characteristics:** Similar to the MPL but developed by the Eclipse Foundation. Requires that modifications to licensed code be made available under the EPL, but allows linking with proprietary code.
9. **ISC License:**
    - **Type:** Permissive
    - **Key Characteristics:** A simpler version of the MIT license that removes conditions that are implied by law.
10. **Artistic License 2.0:**
    - **Type:** Permissive/Copyleft (depending on the scenario)
    - **Key Characteristics:** Allows redistribution and modification but has specific terms designed to ensure the original author retains some control over the development of official versions of the software.

These licenses are adapted to different goals and strategies for software distribution and use, from open academic collaboration to commercial proprietary development with open-source components.

![Untitled](Subatomic%20Walkthrough/Untitled%203.png)

![Untitled](Subatomic%20Walkthrough/Untitled%204.png)

![Untitled](Subatomic%20Walkthrough/Untitled%205.png)

![Untitled](Subatomic%20Walkthrough/Untitled%206.png)

**Task 5**

**The malware connects back to a C2 address during execution. What is the domain used for C2?**

- This answer can be found with FakeNetNG, Wireshark, etc…

![Untitled](Subatomic%20Walkthrough/Untitled%207.png)

**Task 6**

**The malware attempts to get the public IP address of an infected system. What is the full URL used to retrieve this information?**

- At this point I need dive into the ASAR file dropped by the executable.

> 💡 An Electron ASAR is a simple extensive archive format, it stands for Atom Shell Archive Format. It concatenates all files into one and is used for packaging the Electron apps to speed up require() on the distributed apps.
> 

> 🗨️ As far a extracting ASAR file contents, there are various tutorials on this topic. Below is the Stackoverflow I used.
[https://stackoverflow.com/questions/38523617/how-to-unpack-an-asar-file](https://stackoverflow.com/questions/38523617/how-to-unpack-an-asar-file)
> 

![Untitled](Subatomic%20Walkthrough/Untitled%208.png)

- After extracting the ASAR, i opened the folder in Visual Studio Code, to debug the app.js.

> ⚠️ There will be a few packages that won’t work *“dpapi”* and *“sqlite3”* so you will have to reinstall them with npm.
> 
- After starting the debugging process, I immediately paused to see what the program is doing. Right off the bat I immediately noticed the *“newInjection”*  in the call stack.

![Untitled](Subatomic%20Walkthrough/Untitled%209.png)

![Untitled](Subatomic%20Walkthrough/Untitled%2010.png)

- After heading to this function we can see their is a POST request being made to *“hxxps://ipinfo.io/json”,* which is used to gather info on the target such as IP, country, city, and region.

**Task 7**

**The malware is looking for a particular path to connect back on. What is the full URL used for C2 of this malware?**

- Can be found within the same file as the previous question by searching for *“http”*

![Untitled](Subatomic%20Walkthrough/Untitled%2011.png)

**Task 8**

**The malware has a configured `user_id` which is sent to the C2 in the headers or body on every request. What is the key or variable name sent which contains the user_id value?**

- Can be found in the same file as the previous question by searching for *“user_id”*

![Untitled](Subatomic%20Walkthrough/Untitled%2012.png)

**Task 9**

**The malware checks for a number of hostnames upon execution, and if any are found it will terminate. What hostname is it looking for that begins with `arch`?**

- Can be found in the same file as the previous question by searching for *“arch”*

![Untitled](Subatomic%20Walkthrough/Untitled%2013.png)

**Task 10**

**The malware looks for a number of processes when checking if it is running in a VM; however, the malware author has mistakenly made it check for the same process twice. What is the name of this process?**

- Can be found in the same file as the previous question.

![Untitled](Subatomic%20Walkthrough/Untitled%2014.png)

**Task 11**

**The malware has a special function which checks to see if `C:\Windows\system32\cmd.exe` exists. If it doesn't it will write a file from the C2 server to an unusual location on disk using the environment variable `USERPROFILE`. What is the location it will be written to?**

- Can be found in the same file as the previous question by searching for *“USERPROFILE”*

![Untitled](Subatomic%20Walkthrough/Untitled%2015.png)

**Task 12**

**The malware appears to be targeting browsers as much as Discord. What command is run to locate Firefox cookies on the system?**

- Can be found in the same file as the previous question by searching for *“cookies”* or *“Firefox”*

![Untitled](Subatomic%20Walkthrough/Untitled%2016.png)

**Task 13**

**To finally eradicate the malware, Forela needs you to find out what Discord module has been modified by the malware so they can clean it up. What is the Discord module infected by this malware, and what's the name of the infected file?**

- Can be found in the same file as the previous question by searching for *“module”*

![Untitled](Subatomic%20Walkthrough/Untitled%2017.png)