# JS Endpoint Extractor Documentation

## Overview

The `JS Endpoint Extractor` is a JavaScript bookmarklet designed to extract and display all the relative URLs or endpoints present within a webpage's scripts. It can be particularly useful for bug bounty hunters and penetration testers who need to quickly identify potential API endpoints or other relevant URLs embedded in a web page's scripts.

This script extracts paths from:
- Inline JavaScript (`<script>` tags in HTML).
- External script sources (`src` attributes of `<script>` tags).
- The webpage's full HTML source.

### Features
- **Path Extraction**: Scans and extracts all relative URL paths from the page's scripts.
- **Prefix Customization**: Allows you to set a custom prefix (protocol/domain) to view full URLs.
- **Toggle View Mode**: Switch between raw URL paths and clickable links.
- **Filter**: Filter extracted paths for easier navigation.
- **Copy and Download**: Copy filtered results to clipboard or download them as a `.txt` file.

## How It Works
1. **Script Tag Parsing**: The script identifies all `<script>` tags in the HTML and extracts any URLs contained in their `src` attribute or their inline script.
2. **Regex**: A regular expression is used to match paths in URLs embedded within the scripts.
3. **Popup Window**: A new popup window displays the extracted paths, with a user interface to interact with the data.

### Key Components
- **Extract URLs**: The script uses a regular expression to find paths embedded in scripts.
- **Popup Interface**: The popup window contains an interactive interface with options to filter, copy, or download the paths.
- **Custom Prefix**: You can specify whether the URLs should start with `https://`, `http://`, or the current page's domain.

## Features in Detail

### 1. **Prefix Customization**
You can modify the protocol (`https://` or `http://`) and even override the domain by providing a custom domain. This is useful when the extracted paths are relative URLs, and you want to convert them into full URLs.

### 2. **Toggle View Mode**
The extracted paths can be viewed in two modes:
- **Raw URL Mode**: Displays the paths as they are, without linking.
- **Link Mode**: Displays the paths as clickable links, making it easier to navigate to them directly.

### 3. **Filter**
A live filter allows you to quickly search through the list of extracted URLs. This can help you narrow down the results if you're only looking for specific endpoints or patterns.

### 4. **Copy to Clipboard**
You can copy all visible paths to your clipboard with a single click. The paths copied are those that are currently visible (after applying any filters).

### 5. **Download**
You can download the list of visible paths as a `.txt` file. This makes it easy to save the results for further analysis or sharing.

## Installation
To use the script:

### Method 1:
1. **Create a bookmarklet**: Copy the following JavaScript code:

```
javascript:(async()=>{const r=/(?<=(\"|%27|`))\/[a-zA-Z0-9_?&=\/\-#\.]*(?=(\"|'|%60))/g,e=new Set,t=document.getElementsByTagName("script"),n=t=>{const a=t.matchAll(r);for(const t of a)e.add(t[0])};await Promise.all(Array.from(t).map(async t=>{try{t.src?n(await(await fetch(t.src)).text()):n(t.innerHTML)}catch(t){console.warn("Script fetch error:",t)}})),n(document.documentElement.outerHTML);const d=location.origin,paths=[...e],win=window.open("","_blank","width=800,height=600,scrollbars=yes");if(!win)return alert("Popup blocked. Please allow popups.");const style=`<style>body{background:#111;color:#eee;font-family:monospace;padding:1em}button,select,input{margin:0.5em 0;padding:0.5em;background:#222;color:#eee;border:1px solid #555}ul{list-style:none;padding-left:0}li{margin:4px 0}a{color:#4fc3f7;text-decoration:none}a:hover{text-decoration:underline}</style>`,html=`<html><head><title>JS Endpoint Extractor by @DarkLotusKDB</title>${style}</head><body><h2>🔗 Extracted Paths (${paths.length})</h2><label>🌐 Optional Prefix Override: <select id="proto"><option>https://</option><option>http://</option></select><input id="domain" placeholder="Leave blank to use current domain"><button onclick="updatePrefix()">Update</button></label><br><button onclick="toggleMode()">🔄 Toggle View (Basic or URL Mode)</button><br><input id='filt' placeholder='Filter...'><ul id='list'>${paths.map(p=>`<li>${p}</li>`).join("")}</ul><button onclick="copyPaths()">📋 Copy</button> <button onclick="downloadPaths()">💾 Download</button><br><p>Thank you for using JS Endpoint Extractor! Happy hacking! 🤍 <br><a href='https://x.com/DarkLotusKDB' target='_blank'>@DarkLotusKDB</a></p><script>let mode='raw',prefix='${d}';const raw=[${paths.map(p=>"`"+p+"`").join(",")}];let links=raw.map(p=>\`<li><a href="\${prefix}\${p}" target="_blank">\${prefix}\${p}</a></li>\`);function updatePrefix(){const proto=document.getElementById('proto').value,dom=document.getElementById('domain').value.trim();prefix=dom?proto+dom:'${d}';links=raw.map(p=>\`<li><a href="\${prefix}\${p}" target="_blank">\${prefix}\${p}</a></li>\`);if(mode==='link')document.getElementById('list').innerHTML=links.join('');}function toggleMode(){const ul=document.getElementById('list');ul.innerHTML=mode==='raw'?links.join(''):raw.map(p=>'<li>'+p+'</li>').join('');mode=mode==='raw'?'link':'raw';}document.getElementById("filt").oninput=()=>{const f=filt.value.toLowerCase();list.querySelectorAll("li").forEach(l=>{const txt=l.innerText.toLowerCase();l.style.display=txt.includes(f)?'':'none'})};function copyPaths(){const visible=[...document.querySelectorAll('li')].filter(l=>l.style.display!=='none').map(l=>l.innerText).join('\\n');navigator.clipboard.writeText(visible);}function downloadPaths(){const visible=[...document.querySelectorAll('li')].filter(l=>l.style.display!=='none').map(l=>l.innerText).join('\\n'),b=new Blob([visible],{type:'text/plain'}),u=URL.createObjectURL(b),a=document.createElement('a');a.href=u;a.download='paths.txt';a.click();URL.revokeObjectURL(u);}</script></body></html>`;win.document.write(html);win.document.close()})();
```
2. Create a new bookmark in your browser and paste this JavaScript code in the URL field.
3. Activate the bookmarklet on any webpage to extract the URLs.

### Method 2: Direct Execution via Console (Alternative)

If you prefer not to use a bookmarklet, you can run the script directly in the browser's developer console.

1. **Open the Developer Tools Console**:
   - **Chrome/Edge/Brave**: Right-click > Inspect > Console tab.
   - **Firefox**: Right-click > Inspect > Console tab.
   - **Safari**: Enable Developer Tools in Preferences, then right-click > Inspect Element > Console tab.

2. **Enable Pasting in Console (if needed)**:
   - **Chrome**: Type `allow pasting` to enable pasting.
   - **Firefox**: Pasting is allowed by default, but may need enabling in settings.

3. **Paste and Execute the Script**:
   - Copy the script from Method 1, paste it into the console, and press Enter.


## Usage Instructions

1. **Run the bookmarklet** on a page where you want to extract endpoints.
2. The script will open a new popup window that displays the extracted paths.
3. Use the options in the popup window to interact with the results:
   - **Update the Prefix**: Adjust the protocol or domain.
   - **Toggle View**: Switch between raw paths and clickable links.
   - **Filter**: Type to filter paths.
   - **Copy or Download**: Copy the filtered results or download them as a `.txt` file.

## Credits

- **Original Concept**: This idea was originally demonstrated by NahamSec on YouTube, where the basic functionality was shown. The JavaScript oneliner has been enhanced and made more functional in this version by @DarkLotusKDB.

## Contributing

Feel free to contribute by submitting issues or pull requests. If you have ideas for new features or improvements, don't hesitate to open an issue or submit a PR.

## Support Me
[BuyMeACoffee](https://www.buymeacoffee.com/darklotus) If you like my work <3

## About Me

* **DarkLotus** - *CyberSecurity Researcher* - [DarkLotusKDB](https://github.com/darklotuskdb)

### Social Media Handles
* [Twitter](https://twitter.com/darklotuskdb)
* [Medium](https://darklotus.medium.com/)
* [Linkedin](https://www.linkedin.com/in/kamaldeepbhati/)
* [Instagram](https://www.instagram.com/kamaldeepbhati/)