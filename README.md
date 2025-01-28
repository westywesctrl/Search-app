# Search-app
Read me
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Search</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        input, select, button { padding: 10px; margin: 5px; width: 80%; max-width: 300px; }
        #history { margin-top: 20px; text-align: left; }
        .history-item { cursor: pointer; display: flex; justify-content: space-between; }
        .delete-button { color: red; cursor: pointer; }
    </style>
</head>
<body>
    <h1>Search the Web</h1>
    <input type="text" id="query" placeholder="Enter search term" oninput="autocorrect()">
    <select id="engine">
        <option value="google">Google</option>
        <option value="bing">Bing</option>
        <option value="duckduckgo">DuckDuckGo</option>
    </select>
    <button onclick="search()">Search</button>

  <div id="history"></div>

   <script>
        // Expanded autocorrect dictionary with common misspellings
        const autocorrectDictionary = {
            "gogle": "google",
            "bingg": "bing",
            "duckdukgo": "duckduckgo",
            "recieve": "receive",
            "definately": "definitely",
            "seperate": "separate",
            "occured": "occurred",
            "untill": "until",
            "calender": "calendar",
            "advice": "advise", // advice is a noun, advise is a verb
            "loose": "lose",
            "their": "there", // common confusion between homophones
            "your": "you're",
            "its": "it's",
            "would of": "would have",
            "could of": "could have",
            "should of": "should have",
            "alot": "a lot",
            "anyway": "anyways", // a common informal usage
            "definately": "definitely",
            "humerous": "humorous",
            "neccessary": "necessary",
            "priviledge": "privilege",
            "restaraunt": "restaurant",
            "tomatoe": "tomato",
            "thier": "their" // more homophones
        };

        function autocorrect() {
            var input = document.getElementById("query").value.toLowerCase();
            if (autocorrectDictionary[input]) {
                document.getElementById("query").value = autocorrectDictionary[input];
            }
        }

        function search() {
            var query = document.getElementById("query").value;
            var engine = document.getElementById("engine").value;
            var searchUrl = "";

            if (engine === "google") {
                searchUrl = "https://www.google.com/search?q=" + encodeURIComponent(query);
            } else if (engine === "bing") {
                searchUrl = "https://www.bing.com/search?q=" + encodeURIComponent(query);
            } else if (engine === "duckduckgo") {
                searchUrl = "https://duckduckgo.com/?q=" + encodeURIComponent(query);
            }

            // Save search to history
            saveToHistory(query);
            window.open(searchUrl, "_blank"); // Opens in a new tab
        }

        function saveToHistory(query) {
            let history = JSON.parse(localStorage.getItem("searchHistory")) || [];
            if (!history.includes(query)) { // Avoid duplicates
                history.push(query);
                localStorage.setItem("searchHistory", JSON.stringify(history));
                displayHistory();
            }
        }

        function displayHistory() {
            let history = JSON.parse(localStorage.getItem("searchHistory")) || [];
            let historyDiv = document.getElementById("history");
            historyDiv.innerHTML = "<h2>Search History</h2>";
            history.forEach(item => {
                let div = document.createElement("div");
                div.classList.add("history-item");
                div.innerHTML = `${item} <span class="delete-button" onclick="deleteHistory('${item}')">X</span>`;
                historyDiv.appendChild(div);
            });
        }

        function deleteHistory(item) {
            let history = JSON.parse(localStorage.getItem("searchHistory")) || [];
            history = history.filter(query => query !== item); // Remove the selected item
            localStorage.setItem("searchHistory", JSON.stringify(history));
            displayHistory(); // Refresh the displayed history
        }

        // Load search history on page load
        window.onload = function() {
            displayHistory();
        };
    </script>
</body>
</html>
