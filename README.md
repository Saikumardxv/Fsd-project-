# Fsd-project-

<!DOCTYPE html>
<html>
<head>
  <title>Notes App</title>
</head>
<body>
  <h2>Notes App</h2>

  <input id="noteInput" placeholder="Write a note">
  <button onclick="addNote()">Add</button>

  <h3>All Notes</h3>
  <ul id="noteList"></ul>

  <script>
    const api = "http://localhost:5000";

    async function loadNotes() {
      const res = await fetch(api + "/notes");
      const data = await res.json();

      const list = document.getElementById("noteList");
      list.innerHTML = "";

      data.forEach(n => {
        const li = document.createElement("li");
        li.innerHTML = n.text + 
        ` <button onclick="deleteNote('${n._id}')">Delete</button>`;
        list.appendChild(li);
      });
    }

    async function addNote() {
      const text = document.getElementById("noteInput").value;

      await fetch(api + "/add", {
        method: "POST",
        headers: {"Content-Type":"application/json"},
        body: JSON.stringify({ text })
      });

      document.getElementById("noteInput").value = "";
      loadNotes();
    }

    async function deleteNote(id) {
      await fetch(api + `/delete/${id}`, { method: "DELETE" });
      loadNotes();
    }

    loadNotes();
  </script>
</body>
</html>
