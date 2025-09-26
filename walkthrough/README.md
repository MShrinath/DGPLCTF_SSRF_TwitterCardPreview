## 🔍 Walkthrough

1. **Spotting the Hidden Clue**
   - While inspecting the HTML source of the site, I noticed an interesting meta tag:
     ```html
     <meta name="dev" content="/flag route detected, access level: restricted">
     ```
   - This hinted at a possible hidden route named `/flag`.

2. **Probing for the Flag**
   - I tried accessing `/flag` directly via the browser, but it returned a **403 Forbidden** error.
   - That confirmed it: the route was likely restricted based on the request origin.

3. **Testing Internal Access via SSRF**
   - Since the app accepts user input and fetches arbitrary URLs server-side, I suspected it was vulnerable to SSRF.
   - I started testing URLs using the homepage form — trying variations like:
     ```
     http://localhost:5000/flag
     http://0.0.0.0:5000/flag
     ```
   - Those didn’t work as expected, possibly due to DNS resolution or loopback behavior.

4. **Success with 127.0.0.1**
   - Finally, I submitted the following URL through the form:
     ```
     http://127.0.0.1:5000/flag
     ```
   - This time, it worked! The server made a request to its own `/flag` route — from the inside — bypassing the IP-based restriction and returning the contents of the flag stored in the environment file.

5. **Viewing the Response**
   - The flag was either shown directly on the result page or visible by clicking “View HTML Source Code” to inspect the raw server response.
